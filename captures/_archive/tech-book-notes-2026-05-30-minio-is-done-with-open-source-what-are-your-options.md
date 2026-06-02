# MinIO Is Done With Open Source, What Are Your Options?
Source: https://itsfoss.com/news/minio-moves-away-from-open-source/
Captured: 2026-05-30 | Action: read

## Summary
MinIO has officially shifted from open source to a proprietary model, archiving its GitHub repository and ceasing community support. Users must now evaluate open-source alternatives like SeaweedFS, Garage, and RustFS for S3-compatible object storage.

## Key Points
- MinIO removed community features in 2025, stopped publishing binaries, and declared maintenance mode by December 2025.
- No new features, security patches, or compatibility updates will be provided for the community edition.
- SeaweedFS (Apache 2.0) is the closest drop-in replacement; Garage (AGPLv3) targets geo-distributed use; RustFS (Apache 2.0, alpha) offers performance gains.

## Context & Related Topics
- Elasticsearch's licensing shift to SSPL
- S3-compatible storage ecosystems (e.g., Ceph, Rook)
- Open-source vs. enterprise software sustainability trends

## Action Items
- [ ] Set up SeaweedFS test instance for S3 API validation
- [ ] Benchmark RustFS alpha performance with existing MinIO workloads
- [ ] Review Garage's AGPLv3 compliance for organizational policies
