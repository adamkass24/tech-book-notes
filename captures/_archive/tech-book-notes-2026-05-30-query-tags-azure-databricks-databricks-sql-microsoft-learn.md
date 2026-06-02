# Query tags - Azure Databricks - Databricks SQL | Microsoft Learn
Source: https://learn.microsoft.com/en-us/azure/databricks/sql/user/queries/query-tags
Captured: 2026-05-30 | Action: reference-only

## Summary
Query tags are custom key-value pairs applied to Databricks SQL workloads for grouping, cost tracking, and identifying query sources. They appear in the system.query.history table, Query History UI, and APIs, but must avoid sensitive data and adhere to size/character limits.

## Key Points
- Tags group queries by business context (e.g., team:marketing) and track costs via system.query.history table
- Critical limitations: 10KB session limit, 20 tags per query, 128-character keys/values, no sensitive data in tags
- Reserved keys (e.g., dbt_model_name) auto-populate for dbt runs and cannot be overridden
- Session-level tags apply to all statements; statement-level tags target single queries

## Action Items
- [ ] Avoid including passwords/PII in query tag keys/values per security warning
