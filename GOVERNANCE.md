# Governance Model

This document describes the governance model for all Praxiom Labs open-source projects.

## Table of Contents

- [Overview](#overview)
- [Governance Structure](#governance-structure)
- [Roles and Responsibilities](#roles-and-responsibilities)
- [Decision-Making Process](#decision-making-process)
- [Contribution Path](#contribution-path)
- [RFC Process](#rfc-process)
- [Conflict Resolution](#conflict-resolution)
- [Changes to Governance](#changes-to-governance)

---

## Overview

Praxiom Labs projects follow a **meritocratic governance model** inspired by the Apache Software Foundation and the Rust project. Authority is earned through sustained, high-quality contributions and demonstrated commitment to the project's values.

### Core Principles

1. **Openness**: All decisions are made transparently in public forums
2. **Merit**: Influence is earned through contribution quality, not tenure
3. **Consensus**: Major decisions seek broad agreement before action
4. **Respect**: All participants are treated with dignity per our [Code of Conduct](CODE_OF_CONDUCT.md)

---

## Governance Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                        COMMUNITY                                 │
│  Users, bug reporters, documentation contributors               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       CONTRIBUTORS                               │
│  Anyone with merged contributions to any project                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        REVIEWERS                                 │
│  Trusted contributors with code review permissions              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       MAINTAINERS                                │
│  Project leads with merge authority and release permissions     │
│  Team: @praxiomlabs/maintainers                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     SECURITY TEAM                                │
│  Security-sensitive decisions and vulnerability response        │
│  Team: @praxiomlabs/security                                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## Roles and Responsibilities

### Community Members

Anyone using or interested in Praxiom Labs projects.

**Responsibilities:**
- Participate respectfully in discussions
- Report bugs and request features via issue templates
- Follow the Code of Conduct

**How to participate:**
- Open issues and participate in discussions
- Join [Matrix](https://matrix.to/#/#praxiomlabs:matrix.org) or [Discord](https://discord.gg/praxiomlabs)

### Contributors

Anyone who has had a contribution (code, documentation, or otherwise) merged into a project.

**Responsibilities:**
- Follow contribution guidelines
- Respond to feedback on pull requests
- Help triage issues when able

**Recognition:**
- Listed in Git commit history
- Mentioned in release notes for significant contributions
- Eligible for advancement to Reviewer

### Reviewers

Contributors who have demonstrated consistent, high-quality contributions and understanding of project standards.

**Responsibilities:**
- Review pull requests from other contributors
- Provide constructive, timely feedback
- Help mentor new contributors
- Enforce coding standards and best practices

**Permissions:**
- Approve pull requests (but not merge)
- Triage and label issues
- Close duplicate or invalid issues

**How to become a Reviewer:**
- Demonstrate sustained quality contributions (typically 5+ merged PRs)
- Show understanding of project architecture and coding standards
- Be nominated by a Maintainer and approved by consensus

### Maintainers

Experienced contributors responsible for project direction and health.

**Responsibilities:**
- Merge approved pull requests
- Release new versions
- Define project roadmap and priorities
- Make final decisions on contentious issues
- Ensure security and quality standards
- Respond to security reports within SLA

**Permissions:**
- Write access to repositories
- Merge and release authority
- Admin access to project settings
- Access to security advisories

**How to become a Maintainer:**
- Serve as a Reviewer for extended period (typically 6+ months)
- Demonstrate deep project knowledge and sound judgment
- Be nominated and approved by existing Maintainers (unanimous consent)

### Security Team

Maintainers with additional responsibility for security matters.

**Responsibilities:**
- Triage and respond to security reports
- Coordinate vulnerability disclosure
- Review security-sensitive changes
- Maintain threat model and security documentation

**Permissions:**
- Access to private security advisories
- Required reviewer for security-related files (per CODEOWNERS)

---

## Decision-Making Process

### Everyday Decisions

For routine matters (bug fixes, minor improvements, documentation updates):

1. Open a pull request
2. Receive approval from at least one Reviewer or Maintainer
3. Address any feedback
4. Maintainer merges when CI passes

### Significant Decisions

For larger changes (new features, API changes, architectural decisions):

1. Open an issue or RFC for discussion
2. Gather community feedback
3. Reach rough consensus among Maintainers
4. Proceed with implementation
5. Standard review process

### Major Decisions

For breaking changes, governance changes, or contentious issues:

1. Create formal RFC
2. Minimum 14-day discussion period
3. Seek consensus among all active Maintainers
4. Document decision rationale
5. Announce in release notes

### Consensus Process

We follow **lazy consensus** for most decisions:

1. Proposal is made publicly
2. Reasonable time is allowed for objections
3. If no objections, proposal is accepted
4. If objections arise, discuss until resolved or escalate

**Voting (when consensus fails):**
- Simple majority for non-governance decisions
- 2/3 majority for governance changes
- Maintainers have binding votes; others have advisory votes

---

## Contribution Path

```
Community Member
      │
      ▼ (first merged contribution)
  Contributor
      │
      ▼ (sustained quality contributions, nomination)
   Reviewer
      │
      ▼ (extended service, deep expertise, nomination)
  Maintainer
```

### Nomination Process

1. **Initiator**: Any Maintainer may nominate a Contributor/Reviewer
2. **Discussion**: Private discussion among Maintainers (7 days)
3. **Decision**: Consensus required for Reviewers, unanimous for Maintainers
4. **Notification**: Candidate is notified and invited to accept
5. **Onboarding**: Permissions granted, added to relevant teams

### Stepping Down

Contributors at any level may step down voluntarily:

1. Notify Maintainers of intent
2. Transition any ongoing responsibilities
3. Permissions are removed
4. Public acknowledgment and thanks

### Removal

In rare cases, removal may be necessary:

1. Persistent Code of Conduct violations
2. Sustained inactivity (12+ months with no response)
3. Actions harmful to the project

Process:
1. Private discussion among Maintainers
2. Attempt to reach the individual
3. 2/3 Maintainer vote required for removal
4. Permissions revoked
5. May be reinstated if circumstances change

---

## RFC Process

Significant changes go through our Request for Comments (RFC) process.

### When to Write an RFC

- New major features
- Breaking API changes
- Changes to project governance
- Significant architectural changes
- Deprecation of existing features

### RFC Lifecycle

```
Draft → Discussion → Final Comment Period → Accepted/Rejected → Implemented
```

1. **Draft**: Author creates RFC using issue template
2. **Discussion**: Community provides feedback (minimum 7 days)
3. **Final Comment Period (FCP)**: 7-day period for final objections
4. **Decision**: Maintainers accept, reject, or request revisions
5. **Implementation**: Accepted RFCs are implemented

### RFC Template

RFCs should include:

- **Summary**: One-paragraph explanation
- **Motivation**: Why is this needed?
- **Detailed Design**: Technical specification
- **Drawbacks**: Potential downsides
- **Alternatives**: Other approaches considered
- **Unresolved Questions**: Open issues for discussion

---

## Conflict Resolution

### Technical Disagreements

1. Discuss in the relevant issue or PR
2. Seek input from other contributors
3. If unresolved, Maintainers discuss and decide
4. Decision is documented and final

### Interpersonal Conflicts

1. Parties attempt direct resolution
2. If unsuccessful, contact a Maintainer
3. Maintainer mediates discussion
4. For Code of Conduct violations, see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

### Appeals

Decisions may be appealed by:

1. Requesting reconsideration with new information
2. Escalating to full Maintainer group
3. Maintainer group decision is final

---

## Changes to Governance

This governance document may be amended through:

1. Proposal via RFC or issue
2. 14-day discussion period
3. 2/3 majority of active Maintainers
4. Changes documented with rationale

---

## Current Teams

### Maintainers

Team: [@praxiomlabs/maintainers](https://github.com/orgs/praxiomlabs/teams/maintainers)

See [CODEOWNERS](CODEOWNERS) for file-specific ownership.

### Security Team

Team: [@praxiomlabs/security](https://github.com/orgs/praxiomlabs/teams/security)

Contact: security@praxiomlabs.org

---

## References

- [Apache Software Foundation Governance](https://www.apache.org/foundation/governance/)
- [Rust Governance](https://www.rust-lang.org/governance)
- [Open Source Guides: Leadership and Governance](https://opensource.guide/leadership-and-governance/)
- [Red Hat: Understanding Open Source Governance Models](https://www.redhat.com/en/blog/understanding-open-source-governance-models)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-12-26 | Initial governance document |
