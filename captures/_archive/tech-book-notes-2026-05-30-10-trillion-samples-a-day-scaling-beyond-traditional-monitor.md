# 10 trillion samples a day: Scaling beyond traditional monitoring infra at Databricks | Databricks Blog
Source: https://www.databricks.com/blog/10-trillion-samples-day-scaling-beyond-traditional-monitoring-infra-databricks
Captured: 2026-05-30 | Action: read

## Summary
Databricks scaled its monitoring infrastructure to handle 5 billion active timeseries and 10 trillion daily samples by replacing off-the-shelf tools with Pantheon (a Thanos-based TSDB) and Hydra (lakehouse-based raw data storage), enabling automated scaling, cost efficiency, and unified debugging workflows.

## Key Points
- Pantheon optimized TSDB scalability via tiered storage, memory retention policies, and a control plane for self-healing operations.
- Aggregation reduced cardinality growth by dropping high-churn labels during ingestion, using Telegraf and Dicer for sticky routing.
- Hydra leveraged Databricks Lakehouse for raw data storage, achieving 5-minute freshness at 50x lower cost than Pantheon.
- Unified interfaces preserved existing workflows via Grafana PromQL integration and direct Delta Lake SQL access.

## Context & Related Topics
- Thanos (CNCF project for scalable Prometheus TSDB)
- Prometheus cardinality challenges
- Lakehouse architecture (Delta Lake + Spark)
- Telegraf for metric aggregation
- Multi-cloud monitoring patterns
