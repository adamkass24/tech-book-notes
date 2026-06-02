# Grafana's Kubernetes Monitoring Helm Chart v4 Brings Multiple Fixes - InfoQ
Source: https://www.infoq.com/news/2026/05/kubernetes-monitoring-helm/
Captured: 2026-05-30 | Action: read

## Summary
Grafana Labs released Helm chart v4 for Kubernetes monitoring, addressing six months of development to solve scalability issues in complex deployments. Key changes include structural improvements like converting destinations from lists to maps, restructuring collectors, and optimizing memory usage in log pipelines.

## Key Points
- Destinations now use stable map-based naming, eliminating ordering-dependent configuration issues in GitOps workflows.
- Collectors restructured from hard-coded names to user-defined maps with presets (e.g., clustered, daemonset), removing hidden routing logic.
- Explicit service deployment via `telemetryServices` prevents accidental duplicate deployments of metrics collectors.
- Cluster metrics split into dedicated features (e.g., `hostMetrics`, `costMetrics`), each with focused configuration.
- Memory optimization in log pipelines by removing bulk label application and requiring explicit label promotion.

## Context & Related Topics
- kube-prometheus-stack (prometheus-community Helm chart for self-hosted Prometheus/Grafana stacks)
- InfoQ Kubernetes Monitoring Checklist (2025) for SRE observability practices

## Action Items
- [ ] Migrate existing v3 configurations using Grafana's provided Helm chart migration tool
