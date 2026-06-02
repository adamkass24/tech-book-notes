# Row-Level Security in Fabric Data Warehousing - Microsoft Fabric | Microsoft Learn
Source: https://learn.microsoft.com/en-us/fabric/data-warehouse/row-level-security
Captured: 2026-05-30 | Action: reference-only

## Summary
Row-level security (RLS) in Microsoft Fabric Data Warehouse restricts row access via predicate functions and security policies, applied at the database tier for consistent enforcement across all query methods including Power BI. It operates exclusively on Warehouse and SQL analytics endpoints, using schema-bound predicates to filter data silently without application awareness.

## Key Points
- RLS uses inline table-valued functions as predicates enforced by security policies, filtering rows during SELECT/UPDATE/DELETE operations without application modification.
- SCHEMABINDING=ON (default) bypasses additional permission checks for predicate functions; requires ALTER ANY SECURITY POLICY for policy management.
- Security policies apply to all users including dbo, with strict permissions needed for predicate functions (SELECT/REFERENCES).
- Power BI in Direct Lake mode falls back to Direct Query to respect RLS; disabled policies don't filter data.
- Security risks include side-channel attacks via malicious predicate functions requiring collusion and permission monitoring.
