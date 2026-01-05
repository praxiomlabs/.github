# Dependency Management Policy

This document describes the dependency management policies, Software Bill of Materials (SBOM) generation, and lifecycle practices for all Praxiom Labs projects.

## Table of Contents

- [Overview](#overview)
- [Software Bill of Materials (SBOM)](#software-bill-of-materials-sbom)
- [Dependency Lifecycle Policy](#dependency-lifecycle-policy)
- [Environment Dependencies](#environment-dependencies)
- [Security Scanning](#security-scanning)
- [Updating Dependencies](#updating-dependencies)
- [License Compliance](#license-compliance)

---

## Overview

Praxiom Labs projects follow these core principles for dependency management:

1. **Minimal Dependencies**: We minimize the attack surface by limiting dependencies
2. **Pure Rust**: We prefer pure Rust implementations over system library bindings
3. **Security First**: All dependencies are audited for vulnerabilities
4. **License Compliance**: Only permissively-licensed dependencies are allowed

---

## Software Bill of Materials (SBOM)

### What is an SBOM?

A Software Bill of Materials (SBOM) is a formal, machine-readable inventory of all components in a software product. It lists:

- Direct and transitive dependencies
- Version information
- License details
- Known vulnerabilities
- Supply chain provenance

### Generating SBOMs

We support multiple SBOM formats. Use the tools below to generate SBOMs for Praxiom Labs projects.

#### Using cargo-sbom (Recommended)

Generates both SPDX and CycloneDX formats:

```bash
# Install cargo-sbom
cargo install cargo-sbom

# Generate SPDX SBOM
cargo sbom --output-format spdx_json_2_3 > sbom.spdx.json

# Generate CycloneDX SBOM
cargo sbom --output-format cyclone_dx_json_1_4 > sbom.cdx.json
```

#### Using cargo-cyclonedx

Specialized CycloneDX generator:

```bash
# Install cargo-cyclonedx
cargo install cargo-cyclonedx

# Generate CycloneDX SBOM
cargo cyclonedx --format json > bom.json
```

#### Using cargo-auditable

Embeds dependency information directly into compiled binaries:

```bash
# Install cargo-auditable
cargo install cargo-auditable

# Build with embedded SBOM
cargo auditable build --release

# Extract SBOM from binary (using rust-audit-info)
cargo install rust-audit-info
rust-audit-info target/release/your-binary
```

### SBOM Formats Supported

| Format | Tool | Use Case |
|--------|------|----------|
| [SPDX](https://spdx.dev/) | cargo-sbom | Linux Foundation standard, CRA compliance |
| [CycloneDX](https://cyclonedx.org/) | cargo-sbom, cargo-cyclonedx | OWASP standard, security focus |
| Embedded | cargo-auditable | Binary distribution, container scanning |

### Automated SBOM Generation

For CI/CD pipelines, add SBOM generation to your release workflow:

```yaml
# Example for GitLab CI/CD
sbom:
  stage: release
  script:
    - cargo install cargo-sbom
    - cargo sbom --output-format cyclone_dx_json_1_4 > sbom.cdx.json
    - cargo sbom --output-format spdx_json_2_3 > sbom.spdx.json
  artifacts:
    paths:
      - sbom.cdx.json
      - sbom.spdx.json
```

---

## Dependency Lifecycle Policy

### Version Support

We follow a rolling support model aligned with the Rust ecosystem:

| Support Level | Description | Duration |
|---------------|-------------|----------|
| **Active** | Bug fixes, security patches, new features | Current minor version |
| **Maintenance** | Security patches only | Previous minor version |
| **End of Life** | No support | Older versions |

### MSRV (Minimum Supported Rust Version)

All projects target **Rust 2024 Edition**. Most require **Rust 1.85+**, though some projects have higher requirements (e.g., rust-expect requires 1.88+). Check each project's `Cargo.toml` or README for its specific MSRV.

- MSRV changes are documented in release notes
- MSRV bumps require a minor version increment (0.x → 0.y)
- We use the [MSRV-aware resolver](https://blog.rust-lang.org/2025/01/09/Rust-1.84.0/) for dependency resolution

### Dependency Update Cadence

| Dependency Type | Update Frequency | Review Required |
|-----------------|------------------|-----------------|
| Security patches | Immediate | Automated |
| Minor updates | Weekly (automated PR) | Maintainer approval |
| Major updates | As needed | Full review |

### Breaking Changes

When dependencies introduce breaking changes:

1. Pin to last compatible version temporarily
2. Evaluate migration effort
3. Update with appropriate version bump
4. Document migration path for downstream users

### Deprecation Process

When deprecating a dependency:

1. Announce in release notes
2. Provide migration guide
3. Mark as deprecated in code
4. Remove in next major version

---

## Environment Dependencies

### System Requirements

Praxiom Labs projects are designed to be **pure Rust** with minimal system dependencies:

| Dependency | Required By | Alternative |
|------------|-------------|-------------|
| **None** | Base functionality | N/A |

### Optional System Dependencies

| Dependency | Feature | Purpose |
|------------|---------|---------|
| OpenSSL | `native-tls` feature | Alternative to rustls (not recommended) |

### TLS Implementation

We use **rustls** (pure Rust TLS) by default:

- No OpenSSL dependency required
- Consistent behavior across platforms
- Memory-safe implementation
- Supports TLS 1.2 and TLS 1.3

```toml
# Default (pure Rust TLS)
[dependencies]
mssql-client = "0.3"

# Optional: Use system OpenSSL (not recommended)
[dependencies]
mssql-client = { version = "0.3", features = ["native-tls"], default-features = false }
```

### Platform Support

| Platform | Status | Notes |
|----------|--------|-------|
| Linux (x86_64, aarch64) | Tier 1 | Fully supported |
| macOS (x86_64, aarch64) | Tier 1 | Fully supported |
| Windows (x86_64) | Tier 1 | Fully supported |
| FreeBSD | Tier 2 | Best effort |
| WebAssembly (WASI) | Experimental | mcpkit only |

---

## Security Scanning

### Automated Scanning

Every CI run includes:

```bash
# Check for known vulnerabilities
cargo audit

# Check license compliance and dependency policies
cargo deny check
```

### Tools Used

| Tool | Purpose | Database |
|------|---------|----------|
| [cargo-audit](https://github.com/rustsec/rustsec) | Vulnerability detection | RustSec Advisory Database |
| [cargo-deny](https://github.com/EmbarkStudios/cargo-deny) | License and policy enforcement | Custom policy |
| [Dependabot](https://docs.github.com/en/code-security/dependabot) | Automated updates | GitHub Advisory Database |
| [Renovate](https://docs.renovatebot.com/) | Automated updates | Multiple sources |

### Security Advisory Response

| Severity | Response Time | Action |
|----------|---------------|--------|
| Critical | 24 hours | Immediate patch release |
| High | 7 days | Expedited update |
| Medium | 30 days | Normal release cycle |
| Low | 90 days | Next minor release |

---

## Updating Dependencies

### For Contributors

Before submitting a PR with dependency changes:

```bash
# 1. Audit for vulnerabilities
cargo audit

# 2. Check license compliance
cargo deny check

# 3. Ensure MSRV compatibility
cargo msrv verify

# 4. Run full test suite
cargo test --all-features

# 5. Check for unused dependencies
cargo +nightly udeps
```

### Automated Updates

We accept automated PRs from:

- **Dependabot**: Security updates (auto-merge enabled for patch versions)
- **Renovate**: All dependency updates (review required)

### Manual Updates

For major version updates:

1. Create an issue to discuss the update
2. Review changelog for breaking changes
3. Update code to accommodate changes
4. Update documentation if needed
5. Submit PR with explanation

---

## License Compliance

### Allowed Licenses

We only accept dependencies with these licenses:

| License | SPDX ID | Status |
|---------|---------|--------|
| MIT | MIT | Allowed |
| Apache 2.0 | Apache-2.0 | Allowed |
| BSD 2-Clause | BSD-2-Clause | Allowed |
| BSD 3-Clause | BSD-3-Clause | Allowed |
| ISC | ISC | Allowed |
| Zlib | Zlib | Allowed |
| CC0 1.0 | CC0-1.0 | Allowed |
| Unlicense | Unlicense | Allowed |

### Prohibited Licenses

| License | Reason |
|---------|--------|
| GPL, LGPL, AGPL | Copyleft restrictions |
| SSPL | Not OSI approved |
| Commercial | Not open source |

### Enforcement

License compliance is enforced via `cargo-deny`:

```toml
# deny.toml
[licenses]
allow = [
    "MIT",
    "Apache-2.0",
    "BSD-2-Clause",
    "BSD-3-Clause",
    "ISC",
    "Zlib",
    "CC0-1.0",
    "Unlicense",
]
```

---

## References

- [cargo-sbom on crates.io](https://crates.io/crates/cargo-sbom)
- [CycloneDX Rust](https://github.com/CycloneDX/cyclonedx-rust-cargo)
- [RustSec Advisory Database](https://rustsec.org/)
- [cargo-deny Documentation](https://embarkstudios.github.io/cargo-deny/)
- [SPDX Specification](https://spdx.dev/specifications/)
- [CycloneDX Specification](https://cyclonedx.org/specification/overview/)
- [EU Cyber Resilience Act SBOM Requirements](https://openssf.org/blog/2025/10/22/sboms-in-the-era-of-the-cra-toward-a-unified-and-actionable-framework/)
- [OpenSSF OSPS Baseline](https://baseline.openssf.org/versions/2025-02-25)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial policy document |
