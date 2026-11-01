# FreshDB — Research Sources

This document records the primary technical references used during FreshDB research.

## 1. Cache Freshness

### Paper
Revisiting Cache Freshness for Emerging Real-Time Applications

Authors: Ziming Mao, Rishabh Iyer, Scott Shenker, Ion Stoica.

Year: 2024.

Institution: UC Berkeley / ICSI.

Link:
https://arxiv.org/abs/2412.20221

Research relevance:
- Limitations of fixed TTL.
- Adaptive freshness policies.
- Real-time application requirements.
- Trade-offs between freshness and caching performance.

## 2. Microservice Cache Coherence

### Paper
MuCache: A General Framework for Caching in Microservice Graphs.

Conference: USENIX NSDI 2024.

Authors: Haoran Zhang et al.

Link:
https://www.usenix.org/conference/nsdi24/technical-sessions

Research relevance:
- Dependency graphs.
- Cache coherence.
- Invalidation protocols.
- Correctness guarantees.
- Microservice latency.

## 3. Redis Client-Side Caching

### Official documentation

https://redis.io/docs/latest/develop/clients/client-side-caching/

Detailed reference:
https://redis.io/docs/latest/develop/reference/client-side-caching/

Research relevance:
- Client tracking.
- Invalidation tables.
- Broadcasting.
- Tracking memory overhead.
- Invalidation race conditions.
- Lost connection handling.

## 4. Valkey Client-Side Caching

### Official documentation

https://github.com/valkey-io/valkey-doc/blob/main/topics/client-side-caching.md

Research relevance:
- Redis-compatible tracking.
- Opt-in and opt-out caching.
- Prefix-based broadcasting.
- Invalidation trade-offs.

## 5. Dragonfly Architecture

### Official documentation

https://github.com/dragonflydb/dragonfly/blob/main/docs/df-share-nothing.md

Research relevance:
- Shared-nothing architecture.
- Thread-per-core design.
- Database sharding.
- Fiber-based asynchronous processing.
- Atomic multi-key operations.

## 6. Garnet Architecture

### Microsoft Research

https://www.microsoft.com/en-us/research/project/garnet/

Source code:
https://github.com/microsoft/garnet

Research relevance:
- Thread-scalable storage.
- Tsavorite engine.
- Tiered storage.
- Replication.
- Checkpointing and recovery.
- Extensible cache-store architecture.

## 7. Research Methodology

For every reference, FreshDB research will record:

1. Problem addressed.
2. Existing solution.
3. Architectural approach.
4. Performance trade-offs.
5. Correctness guarantees.
6. Limitations.
7. Potential relevance to FreshDB.

## 8. Research Integrity

- Research claims must be supported by primary sources.
- Existing capabilities must not be presented as original inventions.
- Performance comparisons must use reproducible experiments.
- Unverified assumptions must be marked as hypotheses.
- Research gaps must be validated before implementation.

## Status

Research collection: Initial.

Literature review: In progress.
