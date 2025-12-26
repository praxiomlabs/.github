# Praxiom Labs

[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](https://github.com/praxiomlabs/.github/blob/main/LICENSE)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-3.0-4baaaa.svg)](https://github.com/praxiomlabs/.github/blob/main/CODE_OF_CONDUCT.md)
[![MSRV](https://img.shields.io/badge/MSRV-1.85+-orange.svg)](https://www.rust-lang.org/)

**High-performance Rust infrastructure for AI systems and database connectivity.**

We build open-source tools that help developers integrate AI capabilities with enterprise data systems. All projects are written in Rust, emphasizing type safety, performance, and modern async patterns.

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

---

## Quick Links

| Resource | Link |
|----------|------|
| **mcpkit on crates.io** | [crates.io/crates/mcpkit](https://crates.io/crates/mcpkit) |
| **mssql-client on crates.io** | [crates.io/crates/mssql-client](https://crates.io/crates/mssql-client) |
| **mcpkit API Docs** | [docs.rs/mcpkit](https://docs.rs/mcpkit) |
| **mssql-client API Docs** | [docs.rs/mssql-client](https://docs.rs/mssql-client) |
| **MCP Specification** | [modelcontextprotocol.io](https://modelcontextprotocol.io/specification/2025-11-25) |

## Project Status

All projects are under active development and follow [Semantic Versioning](https://semver.org/):

| Project | Version | Stability | MSRV |
|---------|---------|-----------|------|
| mcpkit | 0.5.x | Pre-1.0, API stabilizing | 1.85 |
| mssql-client | 0.3.x | Pre-1.0, API stabilizing | 1.85 |
| mssql-mcp-server | 0.1.x | Early development | 1.85 |

Pre-1.0 versions may have breaking changes between minor releases. See individual project CHANGELOGs for migration guides.

## Contributing

We welcome contributions from everyone! Whether you're fixing bugs, adding features, improving documentation, or helping with testing.

- **[Contributing Guide](https://github.com/praxiomlabs/.github/blob/main/CONTRIBUTING.md)** — Guidelines for all projects
- **[Good First Issues](https://github.com/search?q=org%3Apraxiomlabs+label%3A%22good+first+issue%22+is%3Aopen&type=issues)** — Great for newcomers
- **[Support](https://github.com/praxiomlabs/.github/blob/main/SUPPORT.md)** — How to get help

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
