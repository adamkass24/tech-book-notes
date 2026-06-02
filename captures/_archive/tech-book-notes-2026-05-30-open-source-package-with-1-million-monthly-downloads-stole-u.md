# Open source package with 1 million monthly downloads stole user credentials - Ars Technica
Source: https://arstechnica.com/security/2026/04/open-source-package-with-1-million-monthly-downloads-stole-user-credentials/
Captured: 2026-05-30 | Action: read

## Summary
A malicious version (0.23.3) of the open-source package `elementary-data` (1M+ monthly downloads) stole user credentials via a hidden payload. Developers urge immediate uninstallation, version pinning, credential rotation, and malware checks to mitigate the supply-chain attack.

## Key Points
- Malicious package 0.23.3 injected a payload executing on systems, leaving credentials exposed via environment variables and .env files.
- Critical steps: uninstall 0.23.3, pin to 0.23.4, delete cache, check for `.trinny-security-update` files, rotate all exposed secrets, and audit CI/CD runners.
- Supply-chain attacks on open-source repos are rising, with GitHub workflows being a common vulnerability vector for malicious PRs.

## Context & Related Topics
- Supply-chain attacks (e.g., SolarWinds, npm package hijacking)
- CI/CD security best practices (secrets management, dependency scanning)
- Dependency vulnerability tools (Snyk, Dependabot)

## Action Items
- [ ] Uninstall `elementary-data==0.23.3` and install `elementary-data==0.23.4`
- [ ] Check all systems for `/tmp/.trinny-security-update` (Linux/macOS) or `%TEMP%\.trinny-security-update` (Windows)
- [ ] Rotate all credentials accessed via environment variables, .env files, and CI/CD secrets
- [ ] Audit CI/CD runners for unauthorized secret usage
