# Getting Help with Praxiom Labs Projects

This document explains how to get help with Praxiom Labs open-source projects.

## Documentation

Start with the project documentation:

| Project | Documentation |
|---------|---------------|
| **mcpkit** | [docs.rs/mcpkit](https://docs.rs/mcpkit) \| [GitHub README](https://github.com/praxiomlabs/mcpkit#readme) |
| **rust-mssql-driver** | [docs.rs/mssql-client](https://docs.rs/mssql-client) \| [GitHub README](https://github.com/praxiomlabs/rust-mssql-driver#readme) |
| **mssql-mcp-server** | [GitHub README](https://github.com/praxiomlabs/mssql-mcp-server#readme) |

Each project includes:
- API documentation on docs.rs
- Getting started guides
- Example code in `examples/` directories
- Architecture documentation (where applicable)

## Community Channels

Join our community for real-time discussions, help, and collaboration:

| Platform | Link | Best For |
|----------|------|----------|
| **Matrix** | [#praxiomlabs:matrix.org](https://matrix.to/#/#praxiomlabs:matrix.org) | Real-time chat, open protocol (recommended) |
| **Discord** | [Praxiom Labs Discord](https://discord.gg/praxiomlabs) | Real-time chat, voice channels |
| **GitHub Discussions** | [Discussions](https://github.com/orgs/praxiomlabs/discussions) | Long-form Q&A, RFCs, announcements |

### Channel Guidelines

- **#general** — Introductions, general discussion
- **#help** — Technical questions and troubleshooting
- **#mcpkit** — MCP SDK specific discussions
- **#mssql** — SQL Server driver discussions
- **#announcements** — Release notes and updates (read-only)

### Why Matrix and Discord?

We offer both platforms to meet the community where they are:

- **Matrix** is an open, decentralized protocol — ideal for FOSS projects
- **Discord** has broader adoption and familiar UX for many developers
- Channels are bridged where possible for cross-platform communication

## Asking Questions

### GitHub Issues

For bugs, feature requests, and technical questions:

1. **Search existing issues** first — your question may already be answered
2. **Choose the right repository**:
   - [mcpkit issues](https://github.com/praxiomlabs/mcpkit/issues)
   - [rust-mssql-driver issues](https://github.com/praxiomlabs/rust-mssql-driver/issues)
   - [mssql-mcp-server issues](https://github.com/praxiomlabs/mssql-mcp-server/issues)
3. **Use issue templates** when available — they help us help you faster

### What to Include

When asking for help, please provide:

- **Rust version**: Output of `rustc --version`
- **Crate version**: The version in your `Cargo.toml`
- **Operating system**: Linux, macOS, Windows
- **Minimal reproduction**: Smallest code example that shows the issue
- **Error messages**: Full error output, including backtraces if available
- **What you tried**: Steps you've already taken to solve the problem

### Good Question Example

```
Title: Connection fails with TLS error on Azure SQL Database

I'm using mssql-client 0.3.0 on Ubuntu 22.04 with Rust 1.85.

When connecting to Azure SQL Database, I get this error:
[full error message]

My connection string: Server=*.database.windows.net;Database=mydb;...
(credentials redacted)

I've tried:
- Setting TrustServerCertificate=true
- Using Encrypt=strict

Here's my minimal reproduction:
[code snippet]
```

## Frequently Asked Questions

### General

**Q: What Rust version do I need?**

A: All Praxiom Labs projects require Rust 1.85 or later (2024 Edition). Check your version with `rustc --version` and update with `rustup update stable`.

**Q: Are these projects production-ready?**

A: Projects at version 0.x are under active development and may have breaking changes. Check each project's README for production readiness notes. We recommend pinning exact versions in production.

**Q: How do I report a security vulnerability?**

A: Do NOT open a public issue. See our [Security Policy](SECURITY.md) for private reporting instructions.

**Q: How do I generate an SBOM for your crates?**

A: We document SBOM generation in [DEPENDENCIES.md](DEPENDENCIES.md). Quick start:
```bash
cargo install cargo-sbom
cargo sbom --output-format cyclone_dx_json_1_4 > sbom.cdx.json
```
We support both SPDX and CycloneDX formats.

**Q: What is your MSRV policy?**

A: All projects require Rust 1.85+ (2024 Edition). MSRV bumps require a minor version increment. See [VERSIONING.md](VERSIONING.md) for full details.

**Q: How do you handle breaking changes?**

A: For pre-1.0 versions, breaking changes may occur in minor releases with documentation in CHANGELOG. Post-1.0, we follow strict semantic versioning with deprecation cycles. See [VERSIONING.md](VERSIONING.md).

**Q: Do you have a threat model?**

A: Yes! See [THREAT_MODEL.md](THREAT_MODEL.md) for comprehensive STRIDE analysis covering all projects, including trust boundaries, threat actors, and mitigations.

**Q: What is your governance model?**

A: We follow a meritocratic governance model with clear roles (Community → Contributor → Reviewer → Maintainer). See [GOVERNANCE.md](GOVERNANCE.md) for details on decision-making, the RFC process, and how to advance in the project.

**Q: Do you do fuzz testing?**

A: Yes! We use cargo-fuzz with libFuzzer to continuously test protocol parsers and input handlers. See [FUZZING.md](FUZZING.md) for our fuzz testing methodology and targets.

**Q: Which licenses are your dependencies allowed to use?**

A: We only accept permissively-licensed dependencies (MIT, Apache-2.0, BSD, ISC, Zlib, CC0, Unlicense). Copyleft licenses (GPL, LGPL, AGPL) are prohibited. See [DEPENDENCIES.md](DEPENDENCIES.md#license-compliance).

**Q: How do I join the community chat?**

A: Join us on [Matrix](https://matrix.to/#/#praxiomlabs:matrix.org) (recommended for FOSS) or [Discord](https://discord.gg/praxiomlabs). Channels are bridged where possible.

### mcpkit (MCP SDK)

**Q: Which MCP protocol version does mcpkit support?**

A: mcpkit supports MCP 2025-11-25 (the latest specification), with backward compatibility for 2025-06-18, 2025-03-26, and 2024-11-05.

**Q: Can I use mcpkit with async runtimes other than Tokio?**

A: Yes! mcpkit is runtime-agnostic. While examples use Tokio, the SDK works with smol, async-std, or any runtime that implements standard async traits.

**Q: Which transports are supported?**

A: Five transports: stdio, HTTP/SSE, WebSocket, Unix domain sockets, and gRPC.

### rust-mssql-driver (mssql-client)

**Q: How is this different from Tiberius?**

A: Key differences include first-class TDS 8.0 support (SQL Server 2022+ strict encryption), built-in connection pooling, type-state connections for compile-time safety, and Tokio-native design. See the comparison table in the [README](https://github.com/praxiomlabs/rust-mssql-driver#comparison-with-tiberius).

**Q: Does it work with Azure SQL Database?**

A: Yes. The driver supports Azure SQL Database with automatic redirect handling and Azure AD/Managed Identity authentication.

**Q: What authentication methods are supported?**

A: SQL authentication, Azure AD tokens, Managed Identity, Kerberos/GSSAPI (Linux/macOS), and Windows SSPI.

**Q: Why am I getting TLS errors?**

A: Common causes:
- Using `TrustServerCertificate=true` in production (avoid this)
- Certificate hostname mismatch — use `HostNameInCertificate` option
- SQL Server not configured for TLS 1.2+
- For TDS 8.0 strict mode, ensure your certificate is not self-signed

### mssql-mcp-server

**Q: Which MCP clients work with mssql-mcp-server?**

A: Any MCP-compatible client, including Claude Desktop, Cursor, and custom clients built with mcpkit or other MCP SDKs.

**Q: Is it safe to give AI access to my database?**

A: The server includes multi-layer SQL injection protection, but you should:
- Use a read-only database user when possible
- Limit permissions to specific tables/schemas
- Review query patterns and enable logging
- Never expose production databases without proper access controls

**Q: How do I configure mssql-mcp-server for Claude Desktop?**

A: Add the server to your Claude Desktop configuration file (`claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "mssql": {
      "command": "mssql-mcp-server",
      "args": ["--connection-string", "Server=localhost;Database=mydb;..."]
    }
  }
}
```
See the [README](https://github.com/praxiomlabs/mssql-mcp-server#readme) for full configuration options.

### Development & Building

**Q: How do I build the projects from source?**

A: All projects use standard Cargo commands:
```bash
git clone https://github.com/praxiomlabs/<project>
cd <project>
cargo build --release
cargo test
```
Some projects use [Just](https://github.com/casey/just) for additional tasks. Run `just --list` to see available commands.

**Q: Why is my build failing with feature errors?**

A: Some crates require specific features. Check the project's `Cargo.toml` or README for required features:
```bash
cargo build --features "full"  # Example
cargo build --all-features     # Enable all features
```

**Q: How do I run the examples?**

A: Examples are in the `examples/` directory:
```bash
cargo run --example <example_name>
cargo run --example <example_name> --features <required_features>
```

### Troubleshooting

**Q: I'm getting "connection refused" errors. What's wrong?**

A: Common causes:
1. SQL Server not running or not accepting TCP connections
2. Firewall blocking port 1433 (or custom port)
3. SQL Server Browser service not running (for named instances)
4. Incorrect server address or port

**Q: How do I enable debug logging?**

A: Set the `RUST_LOG` environment variable:
```bash
RUST_LOG=debug cargo run        # All debug logs
RUST_LOG=mssql_client=trace     # Trace-level for specific crate
RUST_LOG=mcpkit=debug           # Debug for mcpkit
```

**Q: My queries are slow. How can I diagnose this?**

A: Enable query timing and check:
1. Network latency to SQL Server
2. Query execution plans (use `SET STATISTICS TIME ON` in SQL Server)
3. Connection pool settings (if using pooling)
4. Whether you're reusing connections or creating new ones

**Q: How do I handle connection timeouts?**

A: Configure timeout in your connection string:
```
Connect Timeout=30;Command Timeout=60
```
Or use the builder API to set timeouts programmatically.

### Contributing

**Q: How do I set up the development environment?**

A: See our [Contributing Guide](CONTRIBUTING.md) for full instructions. Quick start:
```bash
rustup update stable
cargo install just cargo-nextest cargo-audit
git clone https://github.com/praxiomlabs/<project>
cd <project>
just setup  # If available, or cargo build
```

**Q: How do I run the test suite?**

A: Most projects support:
```bash
cargo test                    # Run all tests
cargo nextest run            # Faster parallel testing
just test                    # Project-specific test command
```
Some integration tests require a running SQL Server instance.

**Q: My PR is failing CI. What should I check?**

A: Common CI failures:
1. `cargo fmt --check` — Run `cargo fmt` to fix formatting
2. `cargo clippy` — Address all warnings
3. `cargo test` — Ensure all tests pass
4. `cargo audit` — No new security advisories

## Community Resources

### Model Context Protocol

For MCP-related questions not specific to mcpkit:

- [MCP Specification](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP GitHub](https://github.com/modelcontextprotocol)
- [MCP Discord/Community](https://modelcontextprotocol.io/) (check official site for current links)

### SQL Server

For SQL Server questions not specific to rust-mssql-driver:

- [Microsoft SQL Server Documentation](https://docs.microsoft.com/sql/)
- [TDS Protocol Specification](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/)
- [Stack Overflow [sql-server] tag](https://stackoverflow.com/questions/tagged/sql-server)

### Rust

For general Rust questions:

- [The Rust Programming Language Book](https://doc.rust-lang.org/book/)
- [Rust Users Forum](https://users.rust-lang.org/)
- [Rust Discord](https://discord.gg/rust-lang)
- [Stack Overflow [rust] tag](https://stackoverflow.com/questions/tagged/rust)

## Response Expectations

- **Bug reports**: We aim to triage within 7 days
- **Feature requests**: Reviewed monthly; we prioritize based on community impact
- **Questions**: Best-effort response; not guaranteed SLA

## What We Can't Help With

- General Rust programming questions (use community resources above)
- SQL Server administration or query optimization
- Issues in other MCP implementations
- Proprietary or closed-source integrations

## Security Issues

**Do not open public issues for security vulnerabilities.**

See our [Security Policy](SECURITY.md) for responsible disclosure instructions.

## Commercial Support

For commercial support, consulting, or enterprise inquiries:

| Method | Contact |
|--------|---------|
| **Email** | enterprise@praxiomlabs.org |
| **GitHub** | Open an issue with the `support` label |

We offer:

- **Priority support** — Faster response times and dedicated assistance
- **Custom development** — Feature development and integrations
- **Consulting** — Architecture review, performance optimization, security audits
- **Training** — Team workshops on MCP, Rust, and SQL Server integration

For sponsorship opportunities, see our [GitHub Sponsors](https://github.com/sponsors/praxiomlabs) page.
