# Praxiom Labs

**High-performance Rust infrastructure for AI systems and database connectivity.**

We build open-source tools that help developers integrate AI capabilities with enterprise data systems.

## Projects

### [mcpkit](https://github.com/praxiomlabs/mcpkit)

Rust SDK for the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP 2025-11-25).

- Unified `#[mcp_server]` macro for tools, resources, and prompts
- Runtime-agnostic (Tokio, smol)
- 5 transport options: stdio, HTTP/SSE, WebSocket, Unix sockets, gRPC
- Tower-compatible middleware

```toml
[dependencies]
mcpkit = "0.5"
```

[![Crates.io](https://img.shields.io/crates/v/mcpkit.svg)](https://crates.io/crates/mcpkit)
[![docs.rs](https://docs.rs/mcpkit/badge.svg)](https://docs.rs/mcpkit)

---

### [rust-mssql-driver](https://github.com/praxiomlabs/rust-mssql-driver)

High-performance async SQL Server driver for Rust.

- First-class TDS 8.0 support (SQL Server 2022+ strict encryption)
- Built-in connection pooling (no external crate needed)
- Type-state connections for compile-time safety
- Pure Rust TLS via rustls

```toml
[dependencies]
mssql-client = "0.3"
```

[![Crates.io](https://img.shields.io/crates/v/mssql-client.svg)](https://crates.io/crates/mssql-client)
[![docs.rs](https://docs.rs/mssql-client/badge.svg)](https://docs.rs/mssql-client)

---

### [mssql-mcp-server](https://github.com/praxiomlabs/mssql-mcp-server)

MCP server enabling AI assistants to query SQL Server databases.

- Multi-layer SQL injection protection
- Transaction and pinned session support
- Works with Claude Desktop, Cursor, and other MCP clients
- Circuit breaker and retry patterns

---

## Links

- [crates.io packages](https://crates.io/teams/github:praxiomlabs:maintainers)
- [Documentation (docs.rs)](https://docs.rs)

## License

All projects are dual-licensed under MIT or Apache-2.0.
