# Contributing to Praxiom Labs

Thank you for your interest in contributing to Praxiom Labs projects! This document provides general guidelines that apply across all our repositories.

## Code of Conduct

All Praxiom Labs projects follow the [Rust Code of Conduct](https://www.rust-lang.org/policies/code-of-conduct). Please be respectful and constructive in all interactions.

## Our Projects

| Project | Description | Contributing Guide |
|---------|-------------|-------------------|
| [mcpkit](https://github.com/praxiomlabs/mcpkit) | Rust SDK for Model Context Protocol | [CONTRIBUTING.md](https://github.com/praxiomlabs/mcpkit/blob/main/CONTRIBUTING.md) |
| [rust-mssql-driver](https://github.com/praxiomlabs/rust-mssql-driver) | Async SQL Server driver for Rust | [CONTRIBUTING.md](https://github.com/praxiomlabs/rust-mssql-driver/blob/main/CONTRIBUTING.md) |
| [mssql-mcp-server](https://github.com/praxiomlabs/mssql-mcp-server) | MCP server for SQL Server | [CONTRIBUTING.md](https://github.com/praxiomlabs/mssql-mcp-server/blob/main/CONTRIBUTING.md) |

Each project has its own detailed contributing guide with project-specific setup instructions, testing procedures, and coding standards.

## First-Time Contributors

New to open source or to Praxiom Labs projects? Welcome! Here's how to get started:

### Finding Your First Issue

Look for issues labeled:

- **`good first issue`** — Ideal for newcomers; limited scope, well-documented, with mentorship available
- **`help wanted`** — Open for community contributions; may require more experience
- **`documentation`** — Great for getting familiar with a project without diving into complex code

Browse good first issues:
- [mcpkit good first issues](https://github.com/praxiomlabs/mcpkit/labels/good%20first%20issue)
- [rust-mssql-driver good first issues](https://github.com/praxiomlabs/rust-mssql-driver/labels/good%20first%20issue)
- [mssql-mcp-server good first issues](https://github.com/praxiomlabs/mssql-mcp-server/labels/good%20first%20issue)

### What Makes a Good First Issue

Our `good first issue` labels indicate:

- **Limited scope** — Achievable without deep codebase knowledge
- **Clear description** — Steps to reproduce or specific requirements documented
- **Linked resources** — References to relevant code, tests, or similar implementations
- **Mentorship available** — Maintainers will provide guidance and timely reviews

### Your First Contribution Workflow

1. **Comment on the issue** — Let us know you're working on it
2. **Ask questions** — No question is too basic; we're here to help
3. **Start small** — Don't try to solve everything at once
4. **Submit early** — Open a draft PR to get early feedback
5. **Iterate** — Respond to review comments and refine your work

### Tips for Success

- **Read the project README** and relevant documentation first
- **Run the existing tests** to ensure your environment is set up correctly
- **Look at recent merged PRs** to understand the expected style and process
- **Don't be discouraged** by review feedback — it's part of learning

### Need Help?

- Comment on the issue you're working on
- Open a draft PR with questions inline
- Check the project's [SUPPORT.md](SUPPORT.md) for additional resources

## General Guidelines

### Getting Started

1. **Fork** the repository you want to contribute to
2. **Clone** your fork locally
3. **Set up** the development environment (see project-specific guide)
4. **Create a branch** for your changes
5. **Make your changes** following project conventions
6. **Submit a Pull Request**

### Prerequisites

All Praxiom Labs projects are written in Rust and share common tooling:

- **Rust 1.85+** (2024 Edition)
- **Just** command runner ([installation](https://github.com/casey/just#installation))
- **Git** for version control

Most projects use these development tools:

```bash
# Install recommended tools
cargo install cargo-nextest    # Faster test runner
cargo install cargo-audit      # Security audits
cargo install cargo-deny       # License/dependency checks
```

### Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

[optional body]

[optional footer]
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`

**Examples**:
```
feat(transport): add gRPC transport support
fix(auth): handle token refresh edge case
docs(readme): clarify installation steps
```

### Pull Request Process

1. Ensure all tests pass: `cargo test`
2. Check formatting: `cargo fmt --check`
3. Run lints: `cargo clippy --all-features`
4. Update documentation if needed
5. Fill in the PR template completely
6. Wait for CI to pass
7. Address review feedback promptly

### Code Style

- **No unsafe code** without explicit justification and review
- **Comprehensive error handling** with context
- **Documentation** for all public items
- **Tests** for new functionality
- **No warnings** from clippy or the compiler

## Types of Contributions

### Bug Reports

- Use the project's issue template
- Include reproduction steps
- Specify versions (Rust, crate, OS)
- Include relevant error messages

### Feature Requests

- Describe the problem you're solving
- Propose a solution with API examples
- Consider alternatives
- Indicate if you'd implement it

### Documentation

- Fix typos and clarify wording
- Add examples and tutorials
- Improve API documentation
- Translate documentation

### Code Contributions

- Bug fixes with regression tests
- New features with tests and docs
- Performance improvements with benchmarks
- Refactoring with justification

## Additional Policies

For comprehensive guidance on specific topics:

| Policy | Description |
|--------|-------------|
| **[DEPENDENCIES.md](DEPENDENCIES.md)** | SBOM generation, dependency lifecycle, environment requirements |
| **[VERSIONING.md](VERSIONING.md)** | Semantic versioning, MSRV policy, deprecation process |
| **[THREAT_MODEL.md](THREAT_MODEL.md)** | Security threat analysis, trust boundaries, mitigations |

## License

All Praxiom Labs projects are dual-licensed under:

- [MIT License](https://opensource.org/licenses/MIT)
- [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)

By contributing, you agree that your contributions will be licensed under the same terms.

## Getting Help

- **Questions**: Open a Discussion or Issue in the relevant repository
- **Community Chat**: [Matrix](https://matrix.to/#/#praxiomlabs:matrix.org) or [Discord](https://discord.gg/praxiomlabs)
- **Security Issues**: See our [Security Policy](SECURITY.md)
- **General Inquiries**: Open an issue in this `.github` repository

## Recognition

Contributors are recognized in:
- Git commit history
- Release notes and changelogs
- Project README acknowledgments

Thank you for contributing to open-source infrastructure for AI systems!
