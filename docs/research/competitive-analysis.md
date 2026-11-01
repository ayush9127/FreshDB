# FreshDB — Competitive Analysis

## 1. Purpose

This document evaluates existing in-memory data stores and cache engines to identify their capabilities, architectural trade-offs, and potential research opportunities for FreshDB.

The goal is not to reproduce existing database functionality but to investigate whether a distinct freshness-management approach can provide measurable value.

## 2. Redis

### Existing capabilities
- In-memory key-value storage.
- TTL and eviction policies.
- Client-side caching.
- Server-assisted client tracking.
- Invalidation notifications.
- Prefix-based broadcasting.

### Architectural observations
Redis supports two client-side tracking approaches:
1. Default tracking, where the server remembers which clients have accessed particular keys.
2. Broadcasting, where clients subscribe to key prefixes.

### Limitations and trade-offs to investigate
- Tracking-table memory overhead.
- Invalidation notification overhead.
- Client connection failures.
- Race conditions between reads and invalidation messages.
- Handling of derived data dependencies.

Reference:
https://redis.io/docs/latest/develop/reference/client-side-caching/

## 3. Valkey

### Existing capabilities
- Redis-compatible protocol and data structures.
- Client-side caching.
- Key tracking and invalidation.
- Broadcasting mode.
- Opt-in and opt-out tracking.

### Architectural observations
Valkey maintains Redis-compatible tracking mechanisms, including prefix-based invalidation broadcasting.

### Research questions
- How does tracking overhead behave under high client concurrency?
- What are the trade-offs between tracking and broadcasting?
- How can freshness requirements be expressed independently of TTL?

Reference:
https://github.com/valkey-io/valkey-doc/blob/main/topics/client-side-caching.md

## 4. Dragonfly

### Existing capabilities
- Redis and Memcached API compatibility.
- Multithreaded execution.
- Shared-nothing architecture.
- Sharded in-memory storage.
- Asynchronous fiber-based execution.

### Architectural observations
Database shards are assigned to individual threads. Inter-thread communication uses message passing.

### Research questions
- How could dependency tracking work across multiple shards?
- What is the cost of invalidating dependent entries?
- Can freshness management preserve low-latency execution?

Reference:
https://github.com/dragonflydb/dragonfly/blob/main/docs/df-share-nothing.md

## 5. Garnet

### Existing capabilities
- Redis RESP protocol compatibility.
- Thread-scalable storage.
- Tiered storage.
- Checkpointing and recovery.
- Replication and cluster execution.
- Extensible storage architecture.

### Architectural observations
Garnet uses the Tsavorite storage engine with a modular storage API and separate main and object stores.

### Research questions
- How can freshness policies integrate with storage abstractions?
- What are the implications of tiered storage for freshness?
- How can consistency and invalidation work across replicated nodes?

Reference:
https://github.com/microsoft/garnet

## 6. Initial Comparison

| Capability | Redis | Valkey | Dragonfly | Garnet |
|---|---|---|---|---|
| In-memory storage | Yes | Yes | Yes | Yes |
| TTL | Yes | Yes | Yes | Yes |
| Client tracking | Yes | Yes | Compatibility/features vary | Compatibility/features vary |
| Multithreaded architecture | Yes | Yes | Yes | Yes |
| Tiered storage | Limited | Implementation-dependent | Available features vary | Yes |
| Replication | Yes | Yes | Yes | Yes |
| Dependency-aware freshness
