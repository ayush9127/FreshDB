# FreshDB — Freshness Contract Specification

Status: Draft v0.1
Stage: Research and Architecture
Language: C++20

## 1. Purpose

This specification defines how FreshDB will represent,
measure, and enforce data freshness.

The goal is to provide explicit freshness requirements
rather than relying exclusively on fixed TTL values.

This document is a proposed design, not an implemented
consistency guarantee.

## 2. Core Terminology

### 2.1 Cache Entry

A locally stored representation of source data.

### 2.2 Source Version

A version identifier assigned by the authoritative
data source.

### 2.3 Validation Time

The time at which the cached value was last
authoritatively confirmed.

### 2.4 Maximum Staleness

The maximum permitted age of a cached entry since
its last successful authoritative validation.

### 2.5 Dependency

A relationship between a cached entry and another
data item that influences its correctness.

## 3. Freshness Measurement

Define:

Age = Current Monotonic Time - Validated Monotonic Time

An entry becomes ineligible when:

Age > Maximum Staleness

The engine must not use wall-clock adjustments
to calculate elapsed freshness time.

## 4. Important Guarantee Distinction

FreshDB must distinguish:

### Validation Age Bound

The maximum elapsed time since authoritative
validation.

### Source-relative Freshness

Whether the cached value reflects all relevant
committed source updates up to a defined point.

A validation age bound alone cannot guarantee
source-relative freshness.

Missed invalidations, disconnected clients,
and delayed events must be considered.

## 5. Consistency Modes

### 5.1 STRICT

A cached response must satisfy the defined
authoritative consistency requirement.

If the requirement cannot be verified, the engine
must not silently serve an unverified stale value.

### 5.2 BOUNDED

A cached response may be served only while its
validation age remains within the configured bound.

The source update propagation assumptions must
be documented separately.

### 5.3 STALE_WHILE_REVALIDATE

The engine may serve an older value while a
background refresh occurs, subject to an explicit
maximum stale age.

If the configured bound is exceeded, stale serving
must stop.

## 6. Proposed Policy Fields

- consistency_mode
- max_staleness
- source_version
- validated_at
- generation
- dependencies

Final C++ types are not yet selected.

## 7. Dependency Tracking

FreshDB will investigate a directed graph model.

An entry may depend on one or more source records.

When a source changes, affected dependent entries
must be identified.

Research requirements:

- Transitive invalidation.
- Duplicate dependency handling.
- Cycle detection.
- High fan-out behaviour.
- Cross-shard dependencies.
- Concurrent dependency updates.

## 8. Invalidation Event Model

An invalidation event may contain:

- Event identifier.
- Source identifier.
- Source version.
- Sequence number.
- Event generation.
- Event timestamp.

The implementation must investigate:

- Duplicate delivery.
- Out-of-order delivery.
- Lost events.
- Delayed events.
- Reconnection.
- Replay.
- Full resynchronization.

## 9. Correctness Invariants

The implementation must eventually test:

1. An invalidated entry cannot be served as valid.
2. An expired freshness bound cannot be bypassed.
3. Duplicate events do not corrupt state.
4. Older source versions cannot overwrite newer
   confirmed versions.
5. Dependency invalidation propagates correctly.
6. Recovery does not silently assume missing events
   were harmless.
7. Concurrent updates cannot silently violate the
   selected consistency contract.

## 10. Failure Handling

FreshDB must define behaviour for:

- Source database unavailable.
- Invalidation channel disconnected.
- Event history unavailable.
- Refresh failure.
- Concurrent source updates.
- Process restart.
- Clock and timer anomalies.

Fail-open and fail-closed decisions must be
explicitly configurable where appropriate.

## 11. Performance Evaluation

The implementation will be evaluated using:

- Read latency.
- Write latency.
- Throughput.
- Memory overhead.
- Invalidation propagation latency.
- Freshness violation rate.
- Recovery time.
- Dependency graph traversal cost.

Correctness must be measured separately from
performance.

## 12. Initial Non-Goals

The first prototype will not attempt:

- Distributed SQL transactions.
- Automatic consistency across arbitrary databases.
- Full Redis replacement.
- Multi-region consensus.
- Guaranteed zero-staleness without source
  coordination.

## 13. Open Research Questions

RQ1: Which dependency graph representation is most
efficient for common cache workloads?

RQ2: How can invalidation events be recovered safely?

RQ3: How can source versions be validated efficiently?

RQ4: What is the cost of strict versus bounded freshness?

RQ5: How should freshness policies behave during
network partitions?

RQ6: Can dependency tracking scale across multiple
storage shards?

## 14. Status

Specification: Draft v0.1

Implementation: Not started.

Validation: Pending experiments and design review.
