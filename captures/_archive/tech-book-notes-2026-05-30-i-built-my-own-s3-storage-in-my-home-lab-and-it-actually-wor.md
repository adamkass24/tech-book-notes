# I Built My Own S3 Storage in My Home Lab (And It Actually Works) - Virtualization Howto
Source: https://www.virtualizationhowto.com/2026/04/i-built-my-own-s3-storage-in-my-home-lab-and-it-actually-works/
Captured: 2026-05-30 | Action: read

## Summary
The article details the author's migration from MinIO to RustFS for self-hosted S3-compatible object storage in their home lab, driven by MinIO's license change to AGPLv3 and reduced free features. RustFS, built in Rust with S3 compatibility, offers memory safety, no GC latency, and features like versioning, encryption, and lifecycle management.

## Key Points
- MinIO's shift to AGPLv3 and reduced free features prompted the search for alternatives.
- RustFS provides S3 compatibility, Rust-based performance, and features like object versioning, encryption, and storage tiering.
- Easy Docker deployment with minimal configuration enables seamless integration with tools like Velero, Portainer, and AWS CLI.

## Context & Related Topics
- MinIO (previous home lab standard)
- Ceph RGW (considered but rejected for complexity)
- AWS CLI (native S3 integration)
- Velero (Kubernetes backup tool)
- Proxmox Backup Server (S3-compatible backup)

## Action Items
- [ ] Deploy RustFS using Docker Compose with custom volume mounts and credentials.
- [ ] Configure AWS CLI to interact with RustFS using --endpoint-url http://localhost:9000.
- [ ] Migrate existing MinIO backups to RustFS and validate tool integrations (Velero, Portainer).
