# OpenSSF Best Practices Self-Assessment

This document provides a self-assessment of Praxiom Labs projects against the [OpenSSF Best Practices Badge](https://www.bestpractices.dev/) criteria. It serves as a reference for our compliance status and areas for improvement.

## Table of Contents

- [Overview](#overview)
- [Passing Level Criteria](#passing-level-criteria)
- [Silver Level Criteria](#silver-level-criteria)
- [Gold Level Criteria](#gold-level-criteria)
- [OpenSSF Scorecard](#openssf-scorecard)
- [Continuous Improvement](#continuous-improvement)

---

## Overview

### Badge Status

| Project | Target Level | Status |
|---------|--------------|--------|
| mcpkit | Passing | Self-assessed compliant |
| rust-mssql-driver | Passing | Self-assessed compliant |
| mssql-mcp-server | Passing | Self-assessed compliant |

### Assessment Date

Last reviewed: 2025-12-26

---

## Passing Level Criteria

### Basics

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Project website** describes what software does | ✅ Met | [Profile README](profile/README.md) |
| **Project website** provides obtaining info | ✅ Met | crates.io and GitHub links |
| **Project website** provides feedback/contribution info | ✅ Met | [CONTRIBUTING.md](CONTRIBUTING.md), [SUPPORT.md](SUPPORT.md) |
| **Documentation** for installation and usage | ✅ Met | docs.rs, project READMEs |
| **FLOSS license** clearly stated | ✅ Met | MIT/Apache-2.0 dual license |
| **License in standard location** | ✅ Met | LICENSE files in each repo |
| **Documentation** on how to contribute | ✅ Met | [CONTRIBUTING.md](CONTRIBUTING.md) |
| **Bug reporting process** documented | ✅ Met | Issue templates, [SUPPORT.md](SUPPORT.md) |

### Change Control

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Version control** (Git) | ✅ Met | GitHub repositories |
| **Unique version numbering** | ✅ Met | Semantic Versioning, [VERSIONING.md](VERSIONING.md) |
| **Release notes** for each release | ✅ Met | GitHub Releases, CHANGELOG.md |
| **Version-controlled build/installation** | ✅ Met | Cargo.toml, reproducible builds |

### Reporting

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Bug reporting process** | ✅ Met | GitHub Issues with templates |
| **Vulnerability reporting process** | ✅ Met | [SECURITY.md](SECURITY.md), [SECURITY.txt](SECURITY.txt) |
| **Private vulnerability reporting** | ✅ Met | GitHub Security Advisories, security@praxiomlabs.org |

### Quality

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Working build system** | ✅ Met | Cargo (standard Rust build) |
| **Automated test suite** | ✅ Met | `cargo test`, CI integration |
| **Tests added with functionality** | ✅ Met | PR requirements in [CONTRIBUTING.md](CONTRIBUTING.md) |
| **Warning flags enabled** | ✅ Met | Clippy with strict lints |
| **FLOSS analysis tools** used | ✅ Met | cargo-audit, cargo-deny |

### Security

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Secure development knowledge** | ✅ Met | [THREAT_MODEL.md](THREAT_MODEL.md), security practices |
| **Secure design principles** | ✅ Met | Rust memory safety, `#![deny(unsafe_code)]` |
| **Input validation** | ✅ Met | Schema validation, parameterized queries |
| **Cryptography from reputable sources** | ✅ Met | rustls, ring (vetted libraries) |
| **Default secure configuration** | ✅ Met | TLS required by default |
| **Fixed vulnerabilities** disclosure | ✅ Met | Security advisories on GitHub |

### Analysis

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Static analysis** | ✅ Met | Clippy, cargo-audit |
| **Dynamic analysis** (fuzzing) | ✅ Met | [FUZZING.md](FUZZING.md), cargo-fuzz |
| **Compiler warnings addressed** | ✅ Met | `#![deny(warnings)]` policy |

---

## Silver Level Criteria

### Basics (Silver)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Contribution requirements** documented | ✅ Met | [CONTRIBUTING.md](CONTRIBUTING.md) |
| **Legal authorization** (DCO/CLA) | ✅ Met | DCO in [CONTRIBUTING.md](CONTRIBUTING.md#developer-certificate-of-origin-dco) |
| **Governance model** documented | ✅ Met | [GOVERNANCE.md](GOVERNANCE.md) |
| **Code of Conduct** adopted | ✅ Met | Contributor Covenant 3.0 |
| **Code of Conduct** in standard location | ✅ Met | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) |

### Quality (Silver)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Coding standards** documented | ✅ Met | [CONTRIBUTING.md](CONTRIBUTING.md#code-style) |
| **External dependencies** minimized | ✅ Met | [DEPENDENCIES.md](DEPENDENCIES.md#overview) |
| **Dependencies** up-to-date | ✅ Met | Dependabot, Renovate |
| **SBOM** available | ✅ Met | [DEPENDENCIES.md](DEPENDENCIES.md#software-bill-of-materials-sbom) |

### Security (Silver)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Hardening mechanisms** | ✅ Met | ASLR (OS), no unsafe code |
| **Assurance case** documented | ✅ Met | [THREAT_MODEL.md](THREAT_MODEL.md) |
| **Quick security fix delivery** | ✅ Met | [SECURITY.md](SECURITY.md#severity-levels) SLAs |

### Analysis (Silver)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Static analysis** with medium severity | ✅ Met | Clippy (strict mode) |
| **Dynamic analysis** (fuzzing) | ✅ Met | [FUZZING.md](FUZZING.md) |

---

## Gold Level Criteria

### Basics (Gold)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Bus factor ≥ 2** | ⚠️ Partial | Multiple contributors; formalizing maintainer team |
| **Two unassociated contributors** | ⚠️ Partial | Open to community; actively recruiting |

### Quality (Gold)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Statement coverage ≥ 80%** | ⚠️ In Progress | Measuring coverage; fuzzing supplements |
| **Branch coverage ≥ 80%** | ⚠️ In Progress | Measuring coverage |

### Security (Gold)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Cryptographic agility** | ✅ Met | TLS version configuration |
| **2FA for privileged access** | ⚠️ Recommended | GitHub 2FA encouraged |
| **Signed releases** | ⚠️ Planned | cargo-auditable for future releases |

### Analysis (Gold)

| Criterion | Status | Evidence |
|-----------|--------|----------|
| **Dynamic analysis before release** | ✅ Met | Fuzz testing in CI |
| **Security review of changes** | ✅ Met | CODEOWNERS for security files |

---

## OpenSSF Scorecard

In addition to the Best Practices Badge, we track [OpenSSF Scorecard](https://scorecard.dev/) checks:

| Check | Target | Status |
|-------|--------|--------|
| **Binary-Artifacts** | 10/10 | ✅ No binaries in source |
| **Branch-Protection** | 8+/10 | ✅ Protected main branch |
| **CI-Tests** | 10/10 | ✅ Tests in CI |
| **CII-Best-Practices** | 10/10 | ⚠️ Register for badge |
| **Code-Review** | 10/10 | ✅ PR reviews required |
| **Contributors** | 10/10 | ⚠️ Grow contributor base |
| **Dangerous-Workflow** | 10/10 | ✅ No dangerous patterns |
| **Dependency-Update-Tool** | 10/10 | ✅ Dependabot + Renovate |
| **Fuzzing** | 10/10 | ✅ cargo-fuzz integration |
| **License** | 10/10 | ✅ MIT/Apache-2.0 |
| **Maintained** | 10/10 | ✅ Active development |
| **Packaging** | 10/10 | ✅ Published on crates.io |
| **Pinned-Dependencies** | 8+/10 | ✅ Cargo.lock committed |
| **SAST** | 10/10 | ✅ Clippy in CI |
| **Security-Policy** | 10/10 | ✅ SECURITY.md + SECURITY.txt |
| **Signed-Releases** | 8+/10 | ⚠️ Plan: cargo-auditable |
| **Token-Permissions** | 10/10 | ✅ Minimal CI permissions |
| **Vulnerabilities** | 10/10 | ✅ cargo-audit in CI |
| **Webhooks** | 10/10 | ✅ Secure webhook config |

---

## Continuous Improvement

### Current Priorities

1. **Register for OpenSSF Best Practices Badge** at [bestpractices.dev](https://www.bestpractices.dev/)
2. **Achieve Gold criteria**: Increase bus factor, code coverage
3. **Signed releases**: Implement cargo-auditable or similar
4. **Coverage reporting**: Integrate with Codecov or similar

### Monitoring

- **Scorecard**: Run `scorecard --repo=github.com/praxiomlabs/mcpkit` periodically
- **Badge status**: Track at bestpractices.dev dashboard
- **Audit results**: Review cargo-audit output in CI

### Review Schedule

This self-assessment is reviewed:
- Quarterly (routine review)
- After major releases
- When OpenSSF criteria are updated

---

## References

- [OpenSSF Best Practices Badge Program](https://www.bestpractices.dev/)
- [OpenSSF Scorecard](https://scorecard.dev/)
- [FLOSS Best Practices Criteria (All Levels)](https://www.bestpractices.dev/en/criteria)
- [OpenSSF Security Insights Specification](https://github.com/ossf/security-insights-spec)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial self-assessment |
