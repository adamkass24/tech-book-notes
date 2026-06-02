# Inside Netflix’s Graph Abstraction: Handling 650TB of Graph Data in Milliseconds Globally - InfoQ
Source: https://www.infoq.com/news/2026/03/netflix-graph-abstraction/
Captured: 2026-05-30 | Action: read

## Summary
Netflix's Graph Abstraction handles 650TB of graph data with millisecond latency by separating edge connections from properties, leveraging existing infrastructure, and using global replication. It prioritizes predictable performance over complex query flexibility for operational workloads like service topology and social graphs.

## Key Points
- Restricts traversal depth and requires starting nodes to achieve single-digit ms latency for single-hop queries
- Uses EVCache for distributed caching, strict schema enforcement, and write/read-aside strategies to reduce amplification
- Maintains historical graph state via TimeSeries abstraction for temporal analytics and incident analysis
- Built as a layer on existing Key Value and TimeSeries systems, not a standalone graph database

## Context & Related Topics
- Graph databases (e.g., Neo4j) trade-off flexibility for performance
- Distributed caching patterns (e.g., EVCache, Redis)
- Temporal graph modeling for historical analysis
