# Threat Model

This document provides a comprehensive threat model for all Praxiom Labs projects. It follows the [OWASP Threat Modeling](https://owasp.org/www-community/Threat_Modeling) methodology and aligns with the [Open Threat Model (OTM)](https://www.iriusrisk.com/the-open-threat-model-standard) standard.

## Table of Contents

- [Overview](#overview)
- [Assets](#assets)
- [Trust Boundaries](#trust-boundaries)
- [Threat Actors](#threat-actors)
- [Project-Specific Threat Models](#project-specific-threat-models)
  - [mcpkit (MCP SDK)](#mcpkit-mcp-sdk)
  - [rust-mssql-driver](#rust-mssql-driver)
  - [mssql-mcp-server](#mssql-mcp-server)
- [STRIDE Analysis](#stride-analysis)
- [Mitigations](#mitigations)
- [Risk Matrix](#risk-matrix)
- [References](#references)

---

## Overview

Praxiom Labs develops infrastructure components for AI systems and database connectivity. Our threat model considers:

- **Confidentiality**: Protecting credentials, query data, and sensitive information
- **Integrity**: Ensuring data and code are not tampered with
- **Availability**: Maintaining service reliability and preventing denial of service

### Scope

This threat model covers:

| Project | Type | Primary Function |
|---------|------|------------------|
| mcpkit | SDK/Library | MCP protocol implementation |
| rust-mssql-driver | Database Driver | SQL Server connectivity |
| mssql-mcp-server | Server Application | AI-to-database bridge |

---

## Assets

### Critical Assets

| Asset | Description | Sensitivity |
|-------|-------------|-------------|
| Database Credentials | SQL Server authentication tokens | Critical |
| Query Data | Data returned from database queries | High to Critical |
| Connection Strings | Server addresses and auth info | Critical |
| TLS Private Keys | Server-side encryption keys | Critical |
| Session Tokens | MCP session identifiers | High |

### Secondary Assets

| Asset | Description | Sensitivity |
|-------|-------------|-------------|
| Schema Metadata | Database structure information | Medium |
| Query Patterns | Usage analytics and patterns | Low |
| Error Messages | Stack traces and diagnostics | Medium |
| Logs | Application and audit logs | Medium |

---

## Trust Boundaries

```
┌─────────────────────────────────────────────────────────────────┐
│                        UNTRUSTED ZONE                           │
│  ┌──────────────┐                                               │
│  │  AI Client   │ (Claude, Cursor, custom clients)              │
│  │  (MCP Host)  │                                               │
│  └──────┬───────┘                                               │
│         │                                                       │
│ ════════╪═══════════════════════════════════════════════════════│
│         │           TRUST BOUNDARY 1 (MCP Protocol)             │
│ ════════╪═══════════════════════════════════════════════════════│
│         ▼                                                       │
│  ┌──────────────┐                                               │
│  │ mssql-mcp-   │                                               │
│  │   server     │ (validates, sanitizes, rate-limits)           │
│  └──────┬───────┘                                               │
│         │                                                       │
│ ════════╪═══════════════════════════════════════════════════════│
│         │           TRUST BOUNDARY 2 (TDS Protocol)             │
│ ════════╪═══════════════════════════════════════════════════════│
│         ▼                                                       │
│  ┌──────────────┐                                               │
│  │ rust-mssql-  │                                               │
│  │   driver     │ (parameterized queries, TLS)                  │
│  └──────┬───────┘                                               │
│         │                                                       │
└─────────┼───────────────────────────────────────────────────────┘
          │
══════════╪═══════════════════════════════════════════════════════
          │           TRUST BOUNDARY 3 (Network/TLS)
══════════╪═══════════════════════════════════════════════════════
          ▼
   ┌──────────────┐
   │  SQL Server  │  (TRUSTED ZONE)
   │   Database   │
   └──────────────┘
```

---

## Threat Actors

| Actor | Motivation | Capability | Target |
|-------|-----------|-----------|--------|
| **Malicious AI User** | Data exfiltration | Prompt injection, query manipulation | mssql-mcp-server |
| **Network Attacker** | Credential theft, MitM | Protocol-level attacks | TLS/TDS layer |
| **Malicious Dependency** | Supply chain compromise | Code injection | All projects |
| **Insider Threat** | Data theft | Authorized access abuse | Database layer |
| **Automated Scanner** | Vulnerability discovery | Known exploit patterns | All exposed surfaces |

---

## Project-Specific Threat Models

### mcpkit (MCP SDK)

#### Data Flow

```
Client App → mcpkit SDK → Transport Layer → Network → MCP Server
```

#### Threats

| ID | Threat | STRIDE | Severity | Status |
|----|--------|--------|----------|--------|
| MCP-01 | Prompt injection via tool inputs | Tampering | High | Mitigated |
| MCP-02 | Unauthorized tool invocation | Elevation | Medium | Mitigated |
| MCP-03 | Resource enumeration | Info Disclosure | Low | Accepted |
| MCP-04 | Malformed message DoS | Denial of Service | Medium | Mitigated |
| MCP-05 | Insecure transport (non-TLS) | Info Disclosure | High | Documented |
| MCP-06 | Session hijacking | Spoofing | High | Mitigated |

#### Security Controls

- **Input Validation**: All tool inputs validated against schemas
- **Transport Security**: TLS required for production (`wss://`, `https://`)
- **Message Size Limits**: Configurable limits prevent memory exhaustion
- **Schema Enforcement**: JSON Schema validation on all messages
- **Timeout Handling**: Request timeouts prevent hanging connections

### rust-mssql-driver

#### Data Flow

```
Application → mssql-client → TDS Protocol → TLS Layer → SQL Server
```

#### Threats

| ID | Threat | STRIDE | Severity | Status |
|----|--------|--------|----------|--------|
| TDS-01 | SQL injection | Tampering | Critical | Mitigated |
| TDS-02 | Credential exposure in logs | Info Disclosure | High | Mitigated |
| TDS-03 | TLS downgrade attack | Info Disclosure | Critical | Mitigated |
| TDS-04 | Connection string injection | Tampering | High | Mitigated |
| TDS-05 | Memory disclosure via error | Info Disclosure | Medium | Mitigated |
| TDS-06 | Certificate validation bypass | Spoofing | Critical | Documented |
| TDS-07 | Connection pool exhaustion | Denial of Service | Medium | Mitigated |

#### Security Controls

- **Parameterized Queries**: All queries use RPC binding, never string interpolation
- **TDS 8.0 Strict Mode**: Enforces encryption from connection start
- **Credential Sanitization**: Passwords never appear in logs or errors
- **Certificate Validation**: Strict validation by default (opt-out documented)
- **Connection Pooling**: Limits and timeouts prevent exhaustion
- **`zeroize` Support**: Secure memory wiping for credentials

### mssql-mcp-server

#### Data Flow

```
AI Client → MCP Protocol → mssql-mcp-server → mssql-client → SQL Server
```

#### Threats

| ID | Threat | STRIDE | Severity | Status |
|----|--------|--------|----------|--------|
| SRV-01 | AI-driven SQL injection | Tampering | Critical | Mitigated |
| SRV-02 | Prompt injection to exfiltrate | Info Disclosure | Critical | Mitigated |
| SRV-03 | Excessive query resource usage | Denial of Service | High | Mitigated |
| SRV-04 | Schema exposure to AI | Info Disclosure | Medium | Accepted |
| SRV-05 | Transaction isolation bypass | Tampering | High | Mitigated |
| SRV-06 | Credential leakage via AI | Info Disclosure | Critical | Mitigated |

#### Security Controls

- **Multi-Layer SQL Validation**: AST parsing + pattern matching + parameterization
- **Query Allowlisting**: Optional restriction to approved query patterns
- **Read-Only Mode**: Configurable to prevent data modification
- **Rate Limiting**: Prevents query flooding
- **Circuit Breaker**: Fails fast on repeated errors
- **Audit Logging**: All queries logged for review
- **Minimal Permissions**: Encourages least-privilege database users

---

## STRIDE Analysis

### Summary by Category

| Category | Total Threats | Mitigated | Accepted | Documented |
|----------|--------------|-----------|----------|------------|
| **S**poofing | 2 | 2 | 0 | 0 |
| **T**ampering | 5 | 5 | 0 | 0 |
| **R**epudiation | 0 | 0 | 0 | 0 |
| **I**nfo Disclosure | 7 | 5 | 1 | 1 |
| **D**enial of Service | 3 | 3 | 0 | 0 |
| **E**levation of Privilege | 1 | 1 | 0 | 0 |

### Residual Risks

| Risk | Description | Acceptance Rationale |
|------|-------------|---------------------|
| Schema Exposure (MCP-03, SRV-04) | Database schema visible to AI | Required for AI to formulate queries; mitigate via permissions |
| Non-TLS Transport (MCP-05) | Development may use unencrypted | Documented; TLS required for production |
| TrustServerCertificate (TDS-06) | Option to skip cert validation | Documented as insecure; required for some dev scenarios |

---

## Mitigations

### Code-Level Mitigations

| Mitigation | Implementation | Projects |
|------------|---------------|----------|
| `#![deny(unsafe_code)]` | Compile-time enforcement | All |
| Parameterized queries | RPC binding in TDS | rust-mssql-driver, mssql-mcp-server |
| Input validation | Schema validation, type checking | All |
| Error sanitization | No credentials in errors | All |

### Dependency Mitigations

| Mitigation | Tool | Frequency |
|------------|------|-----------|
| Vulnerability scanning | `cargo audit` | Every CI run |
| License compliance | `cargo deny` | Every CI run |
| Dependency review | Dependabot/Renovate | Automated PRs |
| SBOM generation | `cargo-sbom` | On release |

### Operational Mitigations

| Mitigation | Recommendation |
|------------|----------------|
| Least privilege | Use read-only database accounts |
| Network isolation | Restrict SQL Server network access |
| Monitoring | Enable query audit logging |
| Key rotation | Rotate credentials regularly |

---

## Risk Matrix

| Likelihood / Impact | Low | Medium | High | Critical |
|---------------------|-----|--------|------|----------|
| **High** | | MCP-04 | | |
| **Medium** | MCP-03 | TDS-05, TDS-07 | MCP-01, SRV-03 | |
| **Low** | | | MCP-05, TDS-06 | TDS-01, TDS-03, SRV-01, SRV-02 |

*Note: Critical/Low likelihood items are mitigated but would be severe if controls failed.*

---

## Security Testing

### Recommended Tests

| Test Type | Scope | Frequency |
|-----------|-------|-----------|
| Unit tests for validation | Input handlers | Every PR |
| Fuzzing | Protocol parsers | Monthly |
| Dependency audit | All dependencies | Daily (CI) |
| Penetration testing | Full stack | Annually |

### Fuzzing Targets

- TDS protocol parser (rust-mssql-driver)
- MCP message parser (mcpkit)
- SQL query validator (mssql-mcp-server)

---

## Incident Response

If you discover a security vulnerability:

1. **DO NOT** open a public issue
2. Report via [GitHub Security Advisory](https://github.com/praxiomlabs/.github/security/advisories/new)
3. Or email: security@praxiomlabs.org

See [SECURITY.md](SECURITY.md) for full details.

---

## References

- [OWASP Threat Modeling](https://owasp.org/www-community/Threat_Modeling)
- [STRIDE Threat Model](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [Open Threat Model (OTM) Standard](https://www.iriusrisk.com/the-open-threat-model-standard)
- [MCP Security Specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/security)
- [TDS 8.0 Security](https://learn.microsoft.com/en-us/sql/relational-databases/security/networking/tds-8)
- [OWASP SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [RustSec Advisory Database](https://rustsec.org/)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial threat model |

---

*This threat model is reviewed and updated quarterly, or when significant changes are made to any project architecture.*
