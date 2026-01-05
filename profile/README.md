# Praxiom Labs

[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](https://github.com/praxiomlabs/.github/blob/main/LICENSE)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-3.0-4baaaa.svg)](https://github.com/praxiomlabs/.github/blob/main/CODE_OF_CONDUCT.md)
[![MSRV](https://img.shields.io/badge/MSRV-1.85+-orange.svg)](https://www.rust-lang.org/)
[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/placeholder/badge)](https://www.bestpractices.dev/projects/placeholder)
[![OpenSSF Scorecard](https://img.shields.io/ossf-scorecard/github.com/praxiomlabs/mcpkit?label=OpenSSF%20Scorecard)](https://scorecard.dev/viewer/?uri=github.com/praxiomlabs/mcpkit)
[![OpenSSF Security Insights](https://img.shields.io/badge/OpenSSF-Security%20Insights%20v2.0-green.svg)](https://github.com/praxiomlabs/.github/blob/main/security-insights.yml)
[![Threat Model](https://img.shields.io/badge/Threat%20Model-STRIDE-blueviolet.svg)](https://github.com/praxiomlabs/.github/blob/main/THREAT_MODEL.md)
[![Fuzz Testing](https://img.shields.io/badge/Fuzz%20Testing-cargo--fuzz-yellow.svg)](https://github.com/praxiomlabs/.github/blob/main/FUZZING.md)
[![Matrix](https://img.shields.io/badge/Matrix-%23praxiomlabs-000.svg?logo=matrix)](https://matrix.to/#/#praxiomlabs:matrix.org)
[![Discord](https://img.shields.io/badge/Discord-Join-5865F2.svg?logo=discord&logoColor=white)](https://discord.gg/praxiomlabs)

**High-performance Rust libraries for systems programming.**

We build open-source tools for AI integration, database connectivity, and terminal automation. All projects are written in Rust, emphasizing type safety, performance, and modern async patterns.

## Projects

### [mcpkit](https://github.com/praxiomlabs/mcpkit)

Rust SDK for the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP 2025-11-25).

[![Crates.io](https://img.shields.io/crates/v/mcpkit.svg)](https://crates.io/crates/mcpkit)
[![docs.rs](https://docs.rs/mcpkit/badge.svg)](https://docs.rs/mcpkit)
[![Downloads](https://img.shields.io/crates/d/mcpkit.svg)](https://crates.io/crates/mcpkit)

| Feature | Description |
|---------|-------------|
| Unified Macro | Single `#[mcp_server]` for tools, resources, and prompts |
| Runtime Agnostic | Works with Tokio, smol, or any async runtime |
| 5 Transports | stdio, HTTP/SSE, WebSocket, Unix sockets, gRPC |
| Middleware | Tower-compatible layer system |
| Framework Support | Axum, Actix-web, Rocket, Warp integrations |

```toml
[dependencies]
mcpkit = "0.5"
```

---

### [rust-mssql-driver](https://github.com/praxiomlabs/rust-mssql-driver)

High-performance async SQL Server driver for Rust.

[![Crates.io](https://img.shields.io/crates/v/mssql-client.svg)](https://crates.io/crates/mssql-client)
[![docs.rs](https://docs.rs/mssql-client/badge.svg)](https://docs.rs/mssql-client)
[![Downloads](https://img.shields.io/crates/d/mssql-client.svg)](https://crates.io/crates/mssql-client)

| Feature | Description |
|---------|-------------|
| TDS 8.0 | First-class SQL Server 2022+ strict encryption support |
| Connection Pooling | Built-in pool via `mssql-driver-pool` |
| Type-State | Compile-time connection state enforcement |
| Pure Rust TLS | Uses rustls, no OpenSSL dependency |
| Azure Ready | Managed Identity, automatic redirects |

```toml
[dependencies]
mssql-client = "0.3"
```

---

### [mssql-mcp-server](https://github.com/praxiomlabs/mssql-mcp-server)

MCP server enabling AI assistants to query SQL Server databases securely.

| Feature | Description |
|---------|-------------|
| Security | Multi-layer SQL injection protection |
| Transactions | Full transaction and pinned session support |
| Compatibility | Claude Desktop, Cursor, and other MCP clients |
| Resilience | Circuit breaker and retry patterns |

---

### [rust-expect](https://github.com/praxiomlabs/rust-expect)

Modern async-first terminal automation library, inspired by the classic [Expect](https://en.wikipedia.org/wiki/Expect) tool.

[![Crates.io](https://img.shields.io/crates/v/rust-expect.svg)](https://crates.io/crates/rust-expect)
[![docs.rs](https://docs.rs/rust-expect/badge.svg)](https://docs.rs/rust-expect)
[![Downloads](https://img.shields.io/crates/d/rust-expect.svg)](https://crates.io/crates/rust-expect)

| Feature | Description |
|---------|-------------|
| Async-First | Native Tokio integration, non-blocking I/O |
| Cross-Platform | Single API for Unix (PTY) and Windows (ConPTY) |
| Pattern Matching | Literal, regex, and glob patterns with type safety |
| SSH Backend | Connection pooling, resilient sessions, auto-reconnect |
| Screen Emulation | Full VT100 support with visual diff |
| PII Protection | Built-in sensitive data redaction |
| Observability | Prometheus and OpenTelemetry metrics export |

```toml
[dependencies]
rust-expect = "0.1"
```

---

## Getting Started

### Prerequisites

- **Rust 1.85+** (2024 Edition) — Install via [rustup](https://rustup.rs/)
- **Cargo** — Included with Rust

### Quick Start with mcpkit

```rust
use mcpkit::prelude::*;

#[mcp_server]
struct MyServer;

#[mcp_server]
impl MyServer {
    #[tool]
    async fn hello(&self, name: String) -> String {
        format!("Hello, {}!", name)
    }
}
```

### Quick Start with mssql-client

```rust
use mssql_client::Client;

let client = Client::connect("Server=localhost;Database=mydb;...").await?;
let rows = client.query("SELECT * FROM users WHERE id = @p1", &[&1]).await?;
```

### Quick Start with rust-expect

```rust
use rust_expect::prelude::*;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut session = Session::spawn("bash")?;

    session.expect("$ ").await?;
    session.send_line("echo hello").await?;
    session.expect("hello").await?;

    Ok(())
}
```

---

## Quick Links

| Resource | Link |
|----------|------|
| **mcpkit on crates.io** | [crates.io/crates/mcpkit](https://crates.io/crates/mcpkit) |
| **mssql-client on crates.io** | [crates.io/crates/mssql-client](https://crates.io/crates/mssql-client) |
| **rust-expect on crates.io** | [crates.io/crates/rust-expect](https://crates.io/crates/rust-expect) |
| **mcpkit API Docs** | [docs.rs/mcpkit](https://docs.rs/mcpkit) |
| **mssql-client API Docs** | [docs.rs/mssql-client](https://docs.rs/mssql-client) |
| **rust-expect API Docs** | [docs.rs/rust-expect](https://docs.rs/rust-expect) |
| **MCP Specification** | [modelcontextprotocol.io](https://modelcontextprotocol.io/specification/2025-11-25) |
| **Governance Model** | [GOVERNANCE.md](https://github.com/praxiomlabs/.github/blob/main/GOVERNANCE.md) |
| **Dependency Policy** | [DEPENDENCIES.md](https://github.com/praxiomlabs/.github/blob/main/DEPENDENCIES.md) |
| **Versioning Policy** | [VERSIONING.md](https://github.com/praxiomlabs/.github/blob/main/VERSIONING.md) |
| **Threat Model** | [THREAT_MODEL.md](https://github.com/praxiomlabs/.github/blob/main/THREAT_MODEL.md) |
| **Fuzz Testing** | [FUZZING.md](https://github.com/praxiomlabs/.github/blob/main/FUZZING.md) |
| **Security Policy** | [SECURITY.txt](https://github.com/praxiomlabs/.github/blob/main/SECURITY.txt) (RFC 9116) |
| **OpenSSF Self-Assessment** | [OPENSSF_SELF_ASSESSMENT.md](https://github.com/praxiomlabs/.github/blob/main/OPENSSF_SELF_ASSESSMENT.md) |

## Project Status

All projects are under active development and follow [Semantic Versioning](https://semver.org/):

| Project | Version | Stability | MSRV |
|---------|---------|-----------|------|
| mcpkit | 0.5.x | Pre-1.0, API stabilizing | 1.85 |
| mssql-client | 0.3.x | Pre-1.0, API stabilizing | 1.85 |
| mssql-mcp-server | 0.1.x | Early development | 1.85 |
| rust-expect | 0.1.x | Pre-1.0, API stabilizing | 1.88 |

Pre-1.0 versions may have breaking changes between minor releases. See individual project CHANGELOGs for migration guides.

## Community

Join our community for discussions, support, and collaboration:

| Platform | Description |
|----------|-------------|
| [Matrix](https://matrix.to/#/#praxiomlabs:matrix.org) | Open protocol chat (recommended for FOSS) |
| [Discord](https://discord.gg/praxiomlabs) | Real-time chat and voice |
| [GitHub Discussions](https://github.com/orgs/praxiomlabs/discussions) | Long-form Q&A and RFCs |

## Contributing

We welcome contributions from everyone! Whether you're fixing bugs, adding features, improving documentation, or helping with testing.

- **[Contributing Guide](https://github.com/praxiomlabs/.github/blob/main/CONTRIBUTING.md)** — Guidelines for all projects
- **[Good First Issues](https://github.com/search?q=org%3Apraxiomlabs+label%3A%22good+first+issue%22+is%3Aopen&type=issues)** — Great for newcomers
- **[Support](https://github.com/praxiomlabs/.github/blob/main/SUPPORT.md)** — How to get help
- **[Dependency Policy](https://github.com/praxiomlabs/.github/blob/main/DEPENDENCIES.md)** — SBOM and dependency management

## Security

We take security seriously. If you discover a vulnerability:

- **Do NOT** open a public issue
- See our **[Security Policy](https://github.com/praxiomlabs/.github/blob/main/SECURITY.md)** for reporting instructions
- We aim to acknowledge reports within 48 hours

## License

All projects are dual-licensed under your choice of:

- **[MIT License](https://opensource.org/licenses/MIT)**
- **[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)**

## Sponsor

If you find our projects useful, consider [sponsoring us on GitHub](https://github.com/sponsors/praxiomlabs) to support continued development.

[![Sponsor](https://img.shields.io/badge/Sponsor-❤-ea4aaa.svg)](https://github.com/sponsors/praxiomlabs)
