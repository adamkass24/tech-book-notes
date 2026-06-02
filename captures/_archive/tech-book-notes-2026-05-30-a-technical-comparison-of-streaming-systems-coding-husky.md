# A Technical Comparison of Streaming Systems - Coding Husky
Source: https://ericfu.me/en/compare-streaming-systems/
Captured: 2026-05-30 | Action: read

## Summary
This article provides a technical comparison of major streaming systems (Flink, RisingWave, Spark Streaming, ksqlDB, etc.), emphasizing that design diversity reflects application-specific needs rather than inherent superiority. It highlights key differences in state management, checkpointing, and execution models.

## Key Points
- Flink's Chandy-Lamport barrier-based checkpointing enables consistent exactly-once processing, while RisingWave uses frequent 1-second checkpoints with row-based LSM storage for database-like query consistency.
- Spark Streaming relies on micro-batches (D-Streams) for stateful processing, introducing higher recovery costs and latency compared to true stream systems.
- ksqlDB's Kafka-dependent state management (using changelogs and RocksDB) leads to resource inefficiency and data inconsistency risks.
- RisingWave's Rust-based design and built-in storage engine enable lightweight scaling and OLTP-friendly query capabilities, unlike Flink's reliance on external storage.

## Context & Related Topics
- Apache Flink's Table Store (Paimon) for columnar storage
- Materialize's streaming database approach using Differential Dataflow
- MillWheel (Google Dataflow) state decoupling via BigTable
