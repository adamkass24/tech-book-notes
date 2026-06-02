# From REST to GraphQL: Why and How I Made the Switch - DEV Community
Source: https://dev.to/qbentil/from-rest-to-graphql-why-and-how-i-made-the-switch-4c01
Captured: 2026-05-30 | Action: read

## Summary
The author transitioned from REST to GraphQL after encountering limitations with over-fetching and complex endpoint management in REST APIs. GraphQL's flexibility, single-endpoint efficiency, and strong typing resolved these issues while allowing REST to coexist for simpler use cases.

## Key Points
- GraphQL eliminates over-fetching/under-fetching via client-defined queries and a single endpoint
- Strongly-typed schema improves developer experience and reduces runtime errors
- REST and GraphQL can coexist: use REST for simple operations, GraphQL for complex data needs
- Start small by replacing one REST endpoint with GraphQL during transition

## Context & Related Topics
- REST vs GraphQL comparison
- Apollo Client ecosystem
- GraphQL schema design patterns
- API design tradeoffs in microservices

## Action Items
- [ ] Build sample GraphQL server using Apollo Server and Node.js
- [ ] Identify one REST endpoint to migrate to GraphQL in current project
- [ ] Review Apollo Client documentation for frontend integration
