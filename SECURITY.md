# Security Policy

This security policy applies to all Praxiom Labs projects.

## Reporting a Vulnerability

We take security vulnerabilities seriously. If you discover a security issue in any Praxiom Labs project, please report it responsibly.

### How to Report

**DO NOT** open a public GitHub issue for security vulnerabilities.

Instead, use one of these methods:

1. **GitHub Private Vulnerability Reporting** (Preferred)
   - Navigate to the affected repository
   - Go to Security > Advisories > New draft security advisory
   - Or use: https://github.com/praxiomlabs/[repo]/security/advisories/new

2. **Email**
   - Send details to: **security@praxiomlabs.org**

### What to Include

Please provide:

1. **Description**: Clear explanation of the vulnerability
2. **Impact**: What an attacker could achieve
3. **Affected Versions**: Which versions are vulnerable
4. **Reproduction Steps**: How to trigger the issue
5. **Suggested Fix**: If you have ideas (optional)

### Response Timeline

| Stage | Timeline |
|-------|----------|
| Acknowledgment | Within 48 hours |
| Initial Assessment | Within 7 days |
| Status Update | Every 7 days during investigation |
| Fix Development | Based on severity |
| Coordinated Disclosure | After fix is available |

### Severity Levels

| Severity | Response | Examples |
|----------|----------|----------|
| **Critical** | 24-48 hours | RCE, credential exposure, auth bypass |
| **High** | 7 days | SQL injection, privilege escalation |
| **Medium** | 30 days | Information disclosure, DoS |
| **Low** | 90 days | Minor issues with limited impact |

## Supported Versions

Each project maintains its own version support. Generally:

- Latest minor version: Full support
- Previous minor version: Security fixes only
- Older versions: Not supported

See individual project security policies:
- [mcpkit Security](https://github.com/praxiomlabs/mcpkit/blob/main/SECURITY.md)
- [rust-mssql-driver Security](https://github.com/praxiomlabs/rust-mssql-driver/blob/main/SECURITY.md)
- [mssql-mcp-server Security](https://github.com/praxiomlabs/mssql-mcp-server/blob/main/SECURITY.md)

## Security Practices

### Our Commitments

All Praxiom Labs projects follow these security practices:

- **No unsafe code** - Projects use `#![deny(unsafe_code)]` unless explicitly justified
- **Dependency auditing** - Regular `cargo audit` scans in CI
- **License compliance** - `cargo deny` checks for license and advisory issues
- **Minimal dependencies** - We minimize the attack surface
- **Memory safety** - Rust's ownership system prevents common vulnerabilities

### CI Security Checks

Every pull request runs:
- `cargo audit` - RustSec advisory database checks
- `cargo deny` - License and dependency policy enforcement
- Clippy with strict lints
- No new warnings policy

### Credential Handling

- Passwords and tokens are never logged
- `zeroize` feature available for secure memory wiping
- Connection strings are sanitized in error messages
- Secrets are stored in appropriate types (`SecretString`)

## Threat Model

We maintain a comprehensive threat model covering all projects:

- **[THREAT_MODEL.md](THREAT_MODEL.md)** — Full STRIDE analysis with trust boundaries, threat actors, and mitigations

The threat model is reviewed quarterly and updated when significant architectural changes occur.

## Security Considerations by Project

### mcpkit (MCP SDK)

- **Transport Security**: Use TLS (`wss://`, `https://`) in production
- **Tool Validation**: Always validate tool inputs before processing
- **Prompt Injection**: Sanitize user inputs in prompt templates
- See: [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-11-25/basic/security)

### rust-mssql-driver (SQL Server)

- **TLS Required**: TDS 8.0 strict mode enforces encryption
- **Parameterized Queries**: All queries use RPC binding, never string interpolation
- **Certificate Validation**: Proper validation in production environments
- See: [Threat Model](https://github.com/praxiomlabs/rust-mssql-driver/blob/main/SECURITY.md#threat-model)

### mssql-mcp-server

- **SQL Injection Protection**: Multi-layer validation and parameterization
- **Minimal Privileges**: Use read-only database accounts when possible
- **Query Allowlisting**: Consider restricting allowed query patterns

## Disclosure Policy

- We follow coordinated disclosure
- We credit reporters (unless anonymity is requested)
- We aim to release fixes before public disclosure
- We publish security advisories via GitHub

## Security Checklist for Contributors

Before submitting PRs:

- [ ] No new `unsafe` code without justification
- [ ] Input validation for user-provided data
- [ ] No hardcoded credentials or secrets
- [ ] Error messages don't leak sensitive information
- [ ] Dependencies are from trusted sources
- [ ] `cargo audit` passes with no new advisories

## Related Policies

- [THREAT_MODEL.md](THREAT_MODEL.md) — Comprehensive STRIDE threat analysis
- [DEPENDENCIES.md](DEPENDENCIES.md) — SBOM generation and dependency security
- [VERSIONING.md](VERSIONING.md) — Version support and security patch policy

## References

### Internal Resources
- [OpenSSF Security Insights](security-insights.yml) — Machine-readable security metadata

### External Standards
- [Rust Security Guidelines](https://anssi-fr.github.io/rust-guide/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [RustSec Advisory Database](https://rustsec.org/)
- [MCP Security Specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/security)
- [OpenSSF Security Insights Spec](https://github.com/ossf/security-insights-spec)
- [OpenSSF OSPS Baseline](https://baseline.openssf.org/)
- [TDS 8.0 Security](https://learn.microsoft.com/en-us/sql/relational-databases/security/networking/tds-8)
- [CycloneDX SBOM Standard](https://cyclonedx.org/)
- [SPDX SBOM Standard](https://spdx.dev/)
