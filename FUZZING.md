# Fuzz Testing Policy

This document describes the fuzz testing strategy, methodology, and practices for all Praxiom Labs projects.

## Table of Contents

- [Overview](#overview)
- [Why Fuzz Testing?](#why-fuzz-testing)
- [Fuzzing Strategy](#fuzzing-strategy)
- [Fuzz Targets](#fuzz-targets)
- [Tools and Infrastructure](#tools-and-infrastructure)
- [Running Fuzz Tests](#running-fuzz-tests)
- [Corpus Management](#corpus-management)
- [Continuous Fuzzing](#continuous-fuzzing)
- [Handling Discoveries](#handling-discoveries)
- [Contributing Fuzz Targets](#contributing-fuzz-targets)

---

## Overview

Praxiom Labs uses fuzz testing as a critical component of our security and quality assurance strategy. Fuzzing helps discover edge cases, panics, and potential security vulnerabilities that traditional testing may miss.

### Goals

1. **Find panics and crashes** in Rust code before they reach production
2. **Discover logic errors** in parsers and protocol handlers
3. **Validate input handling** across all trust boundaries
4. **Achieve high code coverage** in security-critical paths
5. **Prevent regressions** through corpus-based testing

---

## Why Fuzz Testing?

While Rust's memory safety guarantees prevent many vulnerability classes, fuzzing remains valuable for:

| Issue Type | Rust Prevents? | Fuzzing Finds? |
|------------|----------------|----------------|
| Buffer overflows | Yes | N/A |
| Use-after-free | Yes | N/A |
| Null pointer dereference | Yes | N/A |
| Integer overflows | Partial* | Yes |
| Panics / unwrap failures | No | Yes |
| Logic errors | No | Yes |
| Denial of service | No | Yes |
| Resource exhaustion | No | Yes |
| Protocol violations | No | Yes |

*Integer overflow checking is enabled in debug builds but wraps in release by default.

---

## Fuzzing Strategy

### Priority Targets

We prioritize fuzzing for:

1. **Protocol Parsers**: TDS protocol (rust-mssql-driver), MCP messages (mcpkit)
2. **User Input Handlers**: Query strings, connection strings, tool inputs
3. **Serialization/Deserialization**: JSON, binary protocols
4. **Trust Boundary Crossings**: All inputs from untrusted sources

### Coverage Goals

| Project | Target Coverage | Current Status |
|---------|-----------------|----------------|
| mcpkit (protocol parsing) | 80%+ | Active |
| rust-mssql-driver (TDS parsing) | 80%+ | Active |
| mssql-mcp-server (query validation) | 70%+ | Active |

---

## Fuzz Targets

### mcpkit

| Target | Description | Location |
|--------|-------------|----------|
| `fuzz_mcp_message` | MCP JSON-RPC message parsing | `fuzz/fuzz_targets/mcp_message.rs` |
| `fuzz_json_schema` | JSON Schema validation | `fuzz/fuzz_targets/json_schema.rs` |
| `fuzz_tool_input` | Tool input parameter handling | `fuzz/fuzz_targets/tool_input.rs` |
| `fuzz_transport` | Transport layer message framing | `fuzz/fuzz_targets/transport.rs` |

### rust-mssql-driver

| Target | Description | Location |
|--------|-------------|----------|
| `fuzz_tds_packet` | TDS packet parsing | `fuzz/fuzz_targets/tds_packet.rs` |
| `fuzz_login_response` | Login response handling | `fuzz/fuzz_targets/login_response.rs` |
| `fuzz_row_data` | Row data deserialization | `fuzz/fuzz_targets/row_data.rs` |
| `fuzz_connection_string` | Connection string parsing | `fuzz/fuzz_targets/connection_string.rs` |
| `fuzz_type_conversion` | SQL type conversions | `fuzz/fuzz_targets/type_conversion.rs` |

### mssql-mcp-server

| Target | Description | Location |
|--------|-------------|----------|
| `fuzz_query_validator` | SQL query validation | `fuzz/fuzz_targets/query_validator.rs` |
| `fuzz_parameter_binding` | Parameter binding logic | `fuzz/fuzz_targets/parameter_binding.rs` |

---

## Tools and Infrastructure

### Primary Fuzzer

We use [cargo-fuzz](https://github.com/rust-fuzz/cargo-fuzz) with libFuzzer as the backend.

```bash
# Install cargo-fuzz (requires nightly Rust)
rustup install nightly
cargo +nightly install cargo-fuzz
```

### Additional Tools

| Tool | Purpose | Usage |
|------|---------|-------|
| [cargo-fuzz](https://github.com/rust-fuzz/cargo-fuzz) | Primary fuzzer | `cargo +nightly fuzz run <target>` |
| [honggfuzz-rs](https://github.com/rust-fuzz/honggfuzz-rs) | Alternative fuzzer | Hardware feedback fuzzing |
| [afl.rs](https://github.com/rust-fuzz/afl.rs) | AFL-based fuzzing | Complementary coverage |
| [cargo-geiger](https://github.com/geiger-rs/cargo-geiger) | Unsafe detection | Identify unsafe code in dependencies |
| [cargo-careful](https://github.com/RalfJung/cargo-careful) | Extra UB checks | Run with additional sanitizers |

### Sanitizers

By default, cargo-fuzz enables AddressSanitizer (ASan). We also use:

| Sanitizer | Flag | Purpose |
|-----------|------|---------|
| AddressSanitizer | Default | Memory errors |
| UndefinedBehaviorSanitizer | `RUSTFLAGS="-Zsanitizer=undefined"` | UB detection |
| MemorySanitizer | `RUSTFLAGS="-Zsanitizer=memory"` | Uninitialized memory |

For pure Rust code without `unsafe`, sanitizers can be disabled for 2x performance:

```bash
cargo +nightly fuzz run --sanitizer=none <target>
```

---

## Running Fuzz Tests

### Basic Usage

```bash
# Navigate to project directory
cd mcpkit

# List available fuzz targets
cargo +nightly fuzz list

# Run a specific target
cargo +nightly fuzz run fuzz_mcp_message

# Run with multiple jobs (parallel fuzzing)
cargo +nightly fuzz run fuzz_mcp_message --jobs 4

# Run for specific duration
cargo +nightly fuzz run fuzz_mcp_message -- -max_total_time=3600
```

### Recommended Flags

```bash
# Full recommended command
cargo +nightly fuzz run <target> \
  --jobs $(nproc) \
  -- \
  -max_total_time=3600 \
  -max_len=65536 \
  -timeout=30
```

### Coverage-Guided Fuzzing

```bash
# Generate coverage report
cargo +nightly fuzz coverage <target>

# View coverage in browser
cargo +nightly fuzz coverage <target> --open
```

---

## Corpus Management

### Corpus Location

```
fuzz/
├── corpus/
│   ├── fuzz_mcp_message/      # Seed inputs for each target
│   ├── fuzz_tds_packet/
│   └── ...
├── artifacts/                  # Crash-inducing inputs
│   ├── fuzz_mcp_message/
│   └── ...
└── fuzz_targets/               # Fuzz target source files
```

### Seed Corpus

Maintain high-quality seed inputs:

```bash
# Add known valid inputs to corpus
cp test_data/valid_message.json fuzz/corpus/fuzz_mcp_message/

# Minimize corpus (remove redundant inputs)
cargo +nightly fuzz cmin <target>

# Merge new findings into corpus
cargo +nightly fuzz merge <target>
```

### Corpus Best Practices

1. **Check in seed corpus**: Commit `fuzz/corpus/` to version control
2. **Minimize regularly**: Run `cmin` weekly to prevent corpus bloat
3. **Include edge cases**: Add boundary values, empty inputs, max-length inputs
4. **Derive from tests**: Extract test inputs as corpus seeds

---

## Continuous Fuzzing

### Schedule

| Type | Frequency | Duration | Trigger |
|------|-----------|----------|---------|
| CI Smoke Test | Every PR | 60 seconds per target | Pull request |
| Nightly Fuzzing | Daily | 4 hours per target | Scheduled |
| Deep Fuzzing | Weekly | 24 hours per target | Scheduled |
| Release Fuzzing | Per release | 72 hours per target | Release branch |

### CI Integration

Example GitLab CI configuration:

```yaml
fuzz-smoke:
  stage: test
  image: rust:nightly
  script:
    - cargo install cargo-fuzz
    - for target in $(cargo +nightly fuzz list); do
        cargo +nightly fuzz run $target -- -max_total_time=60;
      done
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"

fuzz-nightly:
  stage: security
  image: rust:nightly
  script:
    - cargo install cargo-fuzz
    - for target in $(cargo +nightly fuzz list); do
        cargo +nightly fuzz run $target --jobs 4 -- -max_total_time=14400;
      done
  rules:
    - if: $CI_PIPELINE_SOURCE == "schedule"
  artifacts:
    paths:
      - fuzz/artifacts/
    when: always
```

### OSS-Fuzz Integration

For high-visibility projects, consider [OSS-Fuzz](https://google.github.io/oss-fuzz/) integration:

1. Submit project to OSS-Fuzz
2. OSS-Fuzz runs continuous fuzzing on Google infrastructure
3. Bugs are reported privately to maintainers
4. Automatic disclosure after 90 days

---

## Handling Discoveries

### When Fuzzing Finds an Issue

1. **Preserve the artifact**: Save the crash-inducing input
   ```bash
   cp fuzz/artifacts/<target>/crash-* security/crashes/
   ```

2. **Reproduce locally**:
   ```bash
   cargo +nightly fuzz run <target> fuzz/artifacts/<target>/crash-xxxxx
   ```

3. **Minimize the input**:
   ```bash
   cargo +nightly fuzz tmin <target> fuzz/artifacts/<target>/crash-xxxxx
   ```

4. **Create a regression test**: Add the minimized input as a test case

5. **Assess severity**:
   - Panic in debug mode only → Low priority
   - Panic in release mode → Medium priority
   - Memory safety issue (in unsafe code) → High/Critical priority

6. **Follow security process**: If security-relevant, follow [SECURITY.md](SECURITY.md)

### Regression Testing

All discovered issues become test cases:

```rust
#[test]
fn regression_issue_123() {
    // Minimized crash input from fuzzing
    let input = include_bytes!("../fuzz/regressions/issue_123.bin");
    let result = parse_message(input);
    // Should not panic; error result is acceptable
    assert!(result.is_err() || result.is_ok());
}
```

---

## Contributing Fuzz Targets

### Adding New Targets

1. Identify security-critical parsing or input handling code
2. Create a new fuzz target:
   ```bash
   cargo +nightly fuzz add <target_name>
   ```
3. Implement the fuzz harness in `fuzz/fuzz_targets/<target_name>.rs`
4. Add seed corpus inputs
5. Run locally to verify target works
6. Submit PR

### Fuzz Target Guidelines

```rust
#![no_main]
use libfuzzer_sys::fuzz_target;
use your_crate::parse_message;

fuzz_target!(|data: &[u8]| {
    // Call the function under test
    // Panics are caught by the fuzzer
    // Return values are ignored (only panics matter)
    let _ = parse_message(data);
});
```

**Best Practices:**

- Keep fuzz targets focused on one function/feature
- Avoid I/O operations (network, filesystem)
- Use `#[cfg(fuzzing)]` for fuzzing-specific code
- Seed corpus with realistic inputs
- Document what the target tests

---

## Metrics and Reporting

### Key Metrics

| Metric | Goal | Measurement |
|--------|------|-------------|
| Coverage | 80%+ on parsers | `cargo +nightly fuzz coverage` |
| Executions/sec | >1000 | Fuzzer output |
| Unique crashes | 0 (after triage) | CI artifacts |
| Corpus size | Minimized | `du -sh fuzz/corpus/` |

### Reporting

Fuzzing findings are tracked in:

- **Private security advisories**: Security-relevant issues
- **GitHub Issues**: Non-security panics and bugs
- **Regression test suite**: All fixed issues

---

## References

- [Rust Fuzz Book](https://rust-fuzz.github.io/book/)
- [cargo-fuzz Tutorial](https://rust-fuzz.github.io/book/cargo-fuzz/tutorial.html)
- [Google OSS-Fuzz](https://google.github.io/oss-fuzz/)
- [LLVM libFuzzer](https://llvm.org/docs/LibFuzzer.html)
- [Fuzzing Rust Code: A Practical Guide](https://www.wzdftpd.net/blog/rust-fuzzers.html)
- [How to Fuzz Rust Code Continuously](https://about.gitlab.com/blog/how-to-fuzz-rust-code/)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial fuzzing policy |
