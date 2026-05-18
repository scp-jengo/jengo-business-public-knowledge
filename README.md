# Jengo Business - Public Knowledge Layer

**Shared knowledge patterns, frameworks, and best practices**

This repository contains generic, reusable knowledge that benefits all Jengo Business instances.

---

## What This Repo Contains

- ✅ Verification patterns (source checking, claim verification, bias detection)
- ✅ Constitutional AI frameworks (L1/L2/L3 detailed documentation)
- ✅ Best practices (journalism, fact-checking, editorial workflows)
- ✅ Generic patterns (information flows, approval workflows)
- ✅ Public research (anonymized learnings from the ecosystem)

## What This Repo Does NOT Contain

- ❌ Organization-specific knowledge
- ❌ Personal patterns
- ❌ Competitive intelligence
- ❌ Private information

---

## Structure

```
jengo-business-public-knowledge/
├── README.md
├── frameworks/
│   ├── three-layer-intelligence.md     # Full L1/L2/L3 framework
│   ├── mesa-optimizer-prevention.md    # Anti-runaway patterns
│   ├── participatory-vs-extractive.md  # One Force analysis
│   └── constitutional-ai-implementation.md
├── patterns/
│   ├── verification/
│   │   ├── source-credibility.md
│   │   ├── claim-verification.md
│   │   ├── bias-detection.md
│   │   └── cross-reference.md
│   ├── workflows/
│   │   ├── approval-workflow.md
│   │   ├── escalation-pattern.md
│   │   └── audit-trail.md
│   └── communication/
│       ├── stakeholder-communication.md
│       └── professional-communication.md
├── best-practices/
│   ├── journalism/
│   ├── fact-checking/
│   └── editorial-standards/
├── contributions/
│   └── README.md  # How to contribute
└── LICENSE  # MIT
```

---

## How Knowledge Flows

```
Individual discovers pattern
    ↓
Contributes to team/department (with approval)
    ↓
Department aggregates and anonymizes
    ↓
Contributes to organization (with approval)
    ↓
Organization generalizes
    ↓
Contributes to PUBLIC (this repo) - benefits everyone
```

All contributions are:
- Anonymized (no author identity)
- Generalized (no org-specific details)
- Validated (proven to work, not theoretical)

---

## Using This Knowledge

When you launch Jengo, it automatically inherits from this repository:

```yaml
# inheritance-chain.yaml
inheritance:
  - name: public-knowledge
    repo: https://github.com/scp-jengo/jengo-business-public-knowledge.git
    branch: main
    auto_sync: daily
```

You can override any pattern with your org/user-specific version.

---

## Contributing

See `CONTRIBUTING.md` for guidelines.

**Requirements for contributions:**
1. Generic (not org-specific)
2. Validated (proven in practice)
3. Anonymized (no PII, no org names)
4. Documented (clear explanation + examples)

---

## Examples

### Verification Pattern

```markdown
# Source Credibility Check

When verifying a news source:

1. Check domain age (whois lookup)
2. Check SSL certificate validity
3. Search for "about us" page
4. Verify contact information
5. Cross-reference with known outlet databases
6. Check for fact-checking organization membership

Success threshold: ≥4 checks passed = credible
```

### Workflow Pattern

```markdown
# Approval Workflow Pattern

For content requiring legal review:

1. Agent flags potential legal risk (defamation, libel)
2. Task moved to "pending_approval" queue
3. Legal reviewer notified (email + Slack)
4. Reviewer approves/rejects with comment
5. If approved → continue workflow
6. If rejected → return to author with feedback
7. All decisions logged to audit trail
```

---

## License

MIT License - See `LICENSE`

This knowledge is freely usable, modifiable, and distributable.

---

## Related Repositories

- **jengo-business-public-identity** - Identity framework
- **jengo-business-public-system** - System implementation
- **jengo-business-public-world** - World model

---

**Status:** Production
**Version:** 1.0.0
**Maintained By:** SCP-Jengo Team + Community Contributions
