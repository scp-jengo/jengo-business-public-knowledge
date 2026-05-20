# How to Contribute

This repository contains generic, validated, anonymized frameworks and patterns. Contributions are welcome from anyone who has identified a pattern worth sharing.

---

## What Belongs Here

A contribution belongs in this repo if:

- **Generic:** it applies to a wide class of situations, not a specific organization, tool, or context. If you need to mention your company name to explain it, it is not generic enough.
- **Validated:** it has been applied in at least one real context and worked. Pure theory is not sufficient. You do not need to prove it works universally — you need to show it worked somewhere real.
- **Anonymized:** no personal identifiers, organization names, client names, or details that would identify the source context. Strip these out before contributing.
- **Documented:** a clear explanation of what the pattern is, when to apply it, and what it is not.

---

## What Does Not Belong Here

- Organization-specific procedures or policies
- Theoretical frameworks with no implementation record
- Frameworks that depend on specific tools or platforms (these belong in tool-specific documentation)
- Opinion or advocacy content
- Content that cannot be reproduced by someone without your context

---

## File Format

Use the standard format from `CONTRIBUTING.md`. The essential sections are:

- **Name and one-sentence description** — at the top, before the first section header
- **Core Principle** — the essential insight, stripped of all unnecessary framing
- **Application** — when to use it, how, with concrete enough examples that someone unfamiliar with the context can apply it
- **Limits** — what this framework does not cover, where it breaks down, what it should not be used for

Footer: `*Version: 1.0 | Jengo Business Public Knowledge*`

See existing frameworks for the quality bar. The bar is: specific enough to act on, general enough to apply beyond the original context.

---

## Contribution Process

1. Fork this repository
2. Create a branch: `contribution/your-pattern-name`
3. Add your file to the appropriate directory (`frameworks/`, `patterns/`, or `best-practices/`)
4. Submit a pull request with:
   - A short description of what the pattern is
   - Where it has been validated (anonymized — "in a newsroom context" not the outlet name)
   - What it explicitly is not (scope limits)

---

## Review Criteria

Contributions are reviewed for:

1. **Does it meet the generic/validated/anonymized/documented requirements?** If not, the PR describes what is missing.
2. **Is it substantively different from existing content?** Partial overlaps are acceptable if the new contribution adds something specific that the existing content does not cover.
3. **Is it specific enough to act on?** Vague principles that sound good but do not guide behavior will be rejected.
4. **Does the Limits section accurately describe what the framework does not cover?** Contributions that overclaim scope will be rejected or revised.

---

## What Gets Rejected

- Contributions that are specific to a platform, tool, or organization
- Contributions that are restatements of existing content
- Contributions that lack a Limits section or whose Limits section says "no limits"
- Contributions where the validation claim cannot be evaluated even in anonymized form

Rejection is not a judgment on the quality of the work — it may simply mean the contribution is too context-specific for this repository. Tool-specific and organization-specific knowledge belongs in the appropriate private or team repositories.

---

*Maintained by the SCP-Jengo community.*
