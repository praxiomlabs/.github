## Description

<!-- Describe your changes in detail -->

## Motivation and Context

<!-- Why is this change required? What problem does it solve? -->
<!-- If it fixes an open issue, please link to the issue here -->

Fixes #

## Related Issues/PRs

<!-- Link to related issues or PRs. Use keywords like "Closes", "Relates to", "Depends on" -->
<!-- Examples:
- Closes #123
- Relates to #456
- Depends on #789
-->

## Type of Change

<!-- Please check the relevant option -->

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Refactoring (no functional changes)
- [ ] Performance improvement
- [ ] Test coverage improvement
- [ ] CI/Build improvement

## Breaking Changes

<!-- If this is a breaking change, describe the impact and migration path -->
<!-- Delete this section if not applicable -->

**What breaks:**

**Migration guide:**

```rust
// Before
old_function();

// After
new_function();
```

## How Has This Been Tested?

<!-- Describe the tests you ran to verify your changes -->
<!-- Provide instructions so reviewers can reproduce -->

- [ ] `cargo test --workspace`
- [ ] `cargo clippy --workspace --all-features`
- [ ] `cargo fmt --check`
- [ ] `cargo doc --no-deps`
- [ ] Manual testing (describe below)

**Test environment:**
- OS:
- Rust version:
- SQL Server version (if applicable):

**Manual test steps:**
1.
2.
3.

## Performance Impact

<!-- If this PR affects performance, describe the impact -->
<!-- Delete this section if not applicable -->

- [ ] No significant performance impact
- [ ] Performance improvement (describe below)
- [ ] Potential performance regression (justified because...)

**Benchmark results (if applicable):**

```
Before:
After:
```

## Security Checklist

<!-- Review these security considerations -->

- [ ] No new `unsafe` code, or unsafe code is justified and documented
- [ ] No hardcoded credentials, API keys, or secrets
- [ ] Sensitive data is not logged or exposed in error messages
- [ ] User inputs are validated before use
- [ ] SQL queries use parameterized statements (no string interpolation)
- [ ] Dependencies are from trusted sources
- [ ] `cargo audit` shows no new vulnerabilities
- [ ] Error messages don't leak internal implementation details

<!-- If any security items don't apply, note why -->

## Code Quality Checklist

- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Public APIs have appropriate visibility (`pub`, `pub(crate)`, etc.)

## Documentation

<!-- Check all that apply -->

- [ ] No documentation changes needed
- [ ] README updated
- [ ] API docs (rustdoc) updated
- [ ] CHANGELOG entry added
- [ ] Migration guide provided (for breaking changes)
- [ ] Examples updated or added

## Screenshots/Output

<!-- If applicable, add screenshots or terminal output to demonstrate changes -->
<!-- Delete this section if not applicable -->

<details>
<summary>Click to expand</summary>

```
// Terminal output or paste screenshots here
```

</details>

## Reviewer Notes

<!-- Optional: Specific areas you'd like reviewers to focus on -->
<!-- Examples:
- Please review the error handling in src/connection.rs
- I'm unsure about the API design in the new module
- This is my first contribution, feedback welcome!
-->

## Checklist Before Requesting Review

- [ ] I have read the [Contributing Guide](https://github.com/praxiomlabs/.github/blob/main/CONTRIBUTING.md)
- [ ] All commits are signed off (`git commit -s`) per the [DCO](https://github.com/praxiomlabs/.github/blob/main/CONTRIBUTING.md#developer-certificate-of-origin-dco)
- [ ] I have checked that this PR is not a duplicate
- [ ] I have added appropriate labels (if I have permission)
- [ ] I am ready for this PR to be reviewed
