# Versioning and Release Policy

This document describes the versioning, release, and deprecation policies for all Praxiom Labs projects.

## Semantic Versioning

All Praxiom Labs projects follow [Semantic Versioning 2.0.0](https://semver.org/):

```
MAJOR.MINOR.PATCH
```

| Component | When Incremented |
|-----------|------------------|
| **MAJOR** | Incompatible API changes |
| **MINOR** | New functionality (backward compatible) |
| **PATCH** | Bug fixes (backward compatible) |

### Pre-1.0 Versions

For versions below 1.0.0 (current state of all projects):

- **0.MINOR.PATCH** indicates development phase
- MINOR bumps may include breaking changes
- PATCH bumps are backward compatible
- API is not yet stable

### Post-1.0 Commitment

Once a project reaches 1.0.0:

- Breaking changes require MAJOR version bump
- Deprecations follow the full deprecation cycle
- At least 6 months notice before removing deprecated APIs

---

## Current Project Versions

| Project | Version | Stability | Target 1.0 |
|---------|---------|-----------|------------|
| mcpkit | 0.5.x | API stabilizing | When MCP spec stabilizes |
| mssql-client | 0.3.x | API stabilizing | Q2 2026 target |
| mssql-mcp-server | 0.1.x | Early development | After mssql-client 1.0 |

---

## Release Cadence

### Regular Releases

| Release Type | Frequency | Notice |
|--------------|-----------|--------|
| Patch (0.x.PATCH) | As needed | Immediate |
| Minor (0.MINOR.0) | ~Monthly | Release notes |
| Major (MAJOR.0.0) | Yearly | 3+ months notice |

### Security Releases

Security patches are released immediately regardless of schedule:

| Severity | Release Timeline |
|----------|------------------|
| Critical | Within 24-48 hours |
| High | Within 7 days |
| Medium | Within 30 days |
| Low | Next regular release |

---

## Support Policy

### Active Support

| Version | Support Level | Duration |
|---------|---------------|----------|
| Latest minor | Full support | Ongoing |
| Previous minor | Security only | 6 months |
| Older versions | No support | N/A |

### Long-Term Support (LTS)

Future 1.0+ releases may include LTS versions:

- LTS versions receive security patches for 2 years
- Only one LTS version active at a time
- LTS versions announced in release notes

---

## MSRV (Minimum Supported Rust Version)

### Current MSRV

All projects: **Rust 1.85+** (2024 Edition)

### MSRV Policy

1. MSRV is documented in `Cargo.toml` (`rust-version` field)
2. MSRV bumps require a minor version increment
3. We support Rust versions for at least 6 months after release
4. We use the [MSRV-aware resolver](https://blog.rust-lang.org/2025/01/09/Rust-1.84.0/)

### Checking MSRV Compatibility

```bash
# Verify MSRV
cargo msrv verify

# Find minimum compatible version
cargo msrv find
```

---

## Deprecation Policy

### Deprecation Process

```
Announcement → Warning → Removal
     │            │          │
     │            │          └── Next major version (or 2 minor for 0.x)
     │            └── Compiler warnings for 1+ release cycle
     └── Release notes + documentation
```

### Deprecation Timeline

| Phase | Pre-1.0 (0.x) | Post-1.0 (1.x+) |
|-------|---------------|-----------------|
| Announcement | Release notes | Release notes |
| Warning period | 1 minor version | 2 minor versions |
| Removal | 2 minor versions | Next major version |

### How We Deprecate

1. **Annotate with `#[deprecated]`**:
   ```rust
   #[deprecated(since = "0.4.0", note = "Use `new_function` instead")]
   pub fn old_function() { ... }
   ```

2. **Document in CHANGELOG**:
   ```markdown
   ### Deprecated
   - `old_function` is deprecated; use `new_function` instead
   ```

3. **Provide migration guide** for significant changes

### Deprecation Exceptions

Some changes may skip the deprecation cycle:

- Security vulnerabilities requiring immediate removal
- Unsound APIs (memory safety issues)
- Compliance with upstream specification changes

---

## Breaking Changes

### What Constitutes a Breaking Change?

| Change Type | Breaking? |
|-------------|-----------|
| Removing public API | Yes |
| Changing function signature | Yes |
| Changing behavior of existing API | Yes |
| Adding required parameters | Yes |
| Bumping MSRV | Yes (minor) |
| Adding new optional features | No |
| Adding new API | No |
| Performance improvements | No |
| Internal refactoring | No |

### Communicating Breaking Changes

1. **CHANGELOG.md** — Document all breaking changes
2. **Release Notes** — Highlight breaking changes prominently
3. **Migration Guide** — Provide step-by-step upgrade instructions
4. **GitHub Release** — Tag with appropriate semver

---

## Release Process

### Pre-Release Checklist

- [ ] All tests passing
- [ ] CHANGELOG.md updated
- [ ] Version bumped in `Cargo.toml`
- [ ] Documentation updated
- [ ] Security audit clean (`cargo audit`)
- [ ] MSRV verified

### Release Artifacts

Each release includes:

| Artifact | Location |
|----------|----------|
| Crate | crates.io |
| Documentation | docs.rs |
| Git tag | GitHub |
| Release notes | GitHub Releases |
| SBOM | GitHub Release assets |

### Version Tags

Git tags follow the format: `v{VERSION}`

Examples:
- `v0.5.0` — Minor release
- `v0.5.1` — Patch release
- `v1.0.0` — Major release

---

## Changelog Format

We follow [Keep a Changelog](https://keepachangelog.com/) format:

```markdown
# Changelog

## [Unreleased]

## [0.5.0] - 2025-12-26

### Added
- New feature description

### Changed
- Changed behavior description

### Deprecated
- API being deprecated

### Removed
- Removed feature (previously deprecated)

### Fixed
- Bug fix description

### Security
- Security fix description
```

---

## Feature Flags

### Stability Tiers

| Tier | Cargo Feature | Stability |
|------|---------------|-----------|
| Stable | Default | Fully supported |
| Beta | `beta` | May change between minor versions |
| Experimental | `unstable` | May change or be removed anytime |

### Using Feature Flags

```toml
[dependencies]
# Stable features only
mcpkit = "0.5"

# Include beta features
mcpkit = { version = "0.5", features = ["beta"] }

# Include experimental features (at your own risk)
mcpkit = { version = "0.5", features = ["unstable"] }
```

---

## Yanking Policy

### When We Yank

Versions are yanked (removed from crates.io) only for:

1. **Security vulnerabilities** — After patch is available
2. **Critical bugs** — Corruption, data loss
3. **Broken builds** — Compilation failures
4. **Accidental publish** — Wrong content published

### Yank Process

1. Publish patched version
2. Yank affected version(s)
3. Announce in GitHub Security Advisory
4. Update CHANGELOG

---

## References

- [Semantic Versioning 2.0.0](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/)
- [Rust API Guidelines — Versioning](https://rust-lang.github.io/api-guidelines/necessities.html)
- [Cargo Book — Publishing](https://doc.rust-lang.org/cargo/reference/publishing.html)
- [RFC 3537 — MSRV Resolver](https://rust-lang.github.io/rfcs/3537-msrv-resolver.html)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial versioning policy |
