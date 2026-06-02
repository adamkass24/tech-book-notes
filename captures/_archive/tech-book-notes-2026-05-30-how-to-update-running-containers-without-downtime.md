# How to Update Running Containers Without Downtime
Source: https://oneuptime.com/blog/post/2026-01-06-docker-update-without-downtime/view
Captured: 2026-05-30 | Action: read

## Summary
The article details strategies for updating Docker containers without downtime, emphasizing health checks, graceful shutdowns, and orchestration techniques like rolling updates, blue-green deployments, and canary releases. Key practices include verifying container readiness before traffic routing and ensuring applications handle termination signals properly.

## Key Points
- Use health checks (e.g., `curl` endpoint) to validate container readiness before routing traffic.
- Configure `update_config.order: start-first` in Docker Compose for seamless container replacement.
- Implement graceful shutdown (SIGTERM handling) to complete in-flight requests before stopping containers.
- Match `stop_grace_period` to application shutdown timeout (e.g., 30s for Node.js).
- Leverage blue-green (Traefik) or weighted traffic (canary) for zero-downtime validation.

## Context & Related Topics
- Docker Swarm (required for full Compose rolling update behavior)
- Traefik reverse proxy (used for routing in blue-green deployments)
- Health check best practices (e.g., dependency verification in `/health` endpoint)

## Action Items
- [ ] Add health check endpoint verifying critical dependencies (DB, cache) in application.
- [ ] Configure `stop_grace_period: 30s` and `update_config.order: start-first` in Docker Compose.
- [ ] Test rolling updates with staged environment using `docker compose up -d --no-deps`.
