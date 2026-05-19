# Knowledge Contribution Best Practices

## Overview

The knowledge repository is a **living system** that grows through operation. Every mistake, every success, every pattern discovered during real work becomes knowledge that guides future agents.

This document defines **how to contribute knowledge effectively**.

## Core Principle

> **Knowledge emerges from operation, not speculation**
>
> We document what we've learned through doing, not what we think might work.

## Types of Knowledge Contributions

### 1. Frameworks (High-Level Concepts)
**What:** Conceptual structures that organize thinking

**When to contribute:**
- You've discovered a repeating pattern across multiple situations
- You have a mental model that consistently helps make decisions
- You've synthesized multiple patterns into a coherent system

**Location:** `frameworks/*.md`

**Example:**
- Three-Layer Intelligence Framework
- Constitutional Reasoning Framework
- Information Flow Control Framework

**Quality bar:**
- Applies to multiple contexts (not one-off)
- Provides decision-making structure
- Integrates with other frameworks

### 2. Patterns (Specific Techniques)
**What:** Concrete approaches to specific problems

**When to contribute:**
- You've solved the same type of problem 3+ times
- You have a reliable technique that works consistently
- You can articulate when to use vs. not use this pattern

**Location:** `patterns/<domain>/*.md`

**Example:**
- Verification patterns (source credibility check, triangulation)
- Error handling patterns
- Communication patterns

**Quality bar:**
- Clearly defines the problem it solves
- Provides step-by-step approach
- Includes when to use / when not to use
- Shows concrete examples

### 3. Best Practices (Operational Guidance)
**What:** Guidelines for effective operation

**When to contribute:**
- You've identified a better way to do common tasks
- You've learned from mistakes and want to prevent recurrence
- You have operational wisdom to share

**Location:** `best-practices/*.md`

**Example:**
- This document
- Conflict resolution guidance
- Quality standards

**Quality bar:**
- Actionable advice
- Explains the "why" not just the "what"
- Addresses common pitfalls

### 4. Anti-Patterns (What Not to Do)
**What:** Common mistakes and why they fail

**When to contribute:**
- You made a mistake and learned from it
- You've observed a recurring failure mode
- You can explain why the approach fails

**Location:** `anti-patterns/*.md`

**Example:**
- Mesa-optimizer anti-pattern
- Sycophantic agreement anti-pattern
- Premature optimization

**Quality bar:**
- Clearly identifies the mistake
- Explains why it's wrong
- Provides the correct alternative

## Contribution Process

### Step 1: Recognize Knowledge
**During operation, ask:**
- Have I solved this before?
- Is this pattern repeating?
- Would future agents benefit from knowing this?
- Have I learned something non-obvious?

**Don't contribute:**
- One-off solutions
- Obvious facts
- Domain-specific trivia
- Speculation without evidence

### Step 2: Extract and Generalize
**Transform specific to general:**

```
SPECIFIC: "Client Acme's auth bug was due to null check missing on line 89"
    ↓
GENERAL: "Authentication flows should validate all inputs before processing,
          including defensive null checks on optional parameters"
```

**Remove context dependencies:**
- Client names → Generic terms
- Specific tech → Technology categories
- Exact errors → Error patterns
- Individual names → Roles

### Step 3: Structure Knowledge
**Use the knowledge-item schema:**

```yaml
id: ""  # Will be generated
type: "pattern"  # framework | pattern | best-practice | anti-pattern
title: "Defensive Null Checking in Authentication"
content: |
  [Detailed explanation]
metadata:
  layer: "device"  # device | user | department | organization | foundation
  status: "stable"  # draft | review | stable | deprecated
  tags: ["authentication", "validation", "defensive-programming"]
  verified: false
application:
  when_to_use: "When handling optional parameters in security-critical code"
  when_not_to_use: "When parameters are guaranteed non-null by schema validation"
evidence:
  empirical: ["Prevented 3 authentication failures in Q1 2026"]
  case_studies: ["Acme Corp auth bug (anonymized)"]
```

### Step 4: Write for Future Agents
**Your audience:** An agent encountering this problem for the first time

**Writing guidelines:**
1. **Start with the problem:** What situation triggers this knowledge?
2. **Explain the solution:** How do you solve it?
3. **Show examples:** Concrete illustrations
4. **Provide decision criteria:** When to use, when not to use
5. **Link to related knowledge:** What else should they read?

**Example structure:**
```markdown
# [Pattern Name]

## Problem
[What situation does this address?]

## Solution
[How to solve it?]

## When to Use
- Situation A
- Situation B

## When NOT to Use
- Situation X (use [other pattern] instead)
- Situation Y (this doesn't apply)

## Example
[Concrete illustration]

## Related
- See also: [Related Pattern]
- Integrates with: [Framework]
```

### Step 5: Self-Review Checklist
Before contributing, verify:

**Content Quality:**
- [ ] Solves a real problem you've encountered
- [ ] Generalized (not client-specific)
- [ ] Actionable (not just theoretical)
- [ ] Complete (includes examples and context)

**Information Flow:**
- [ ] No client names or identifiers
- [ ] No personal information
- [ ] No secrets or credentials
- [ ] No proprietary details

**Integration:**
- [ ] Links to related knowledge items
- [ ] Consistent with existing frameworks
- [ ] Doesn't contradict existing patterns
- [ ] Fits within Three-Layer framework

**Writing Quality:**
- [ ] Clear and concise
- [ ] Structured for scanning (headers, bullets)
- [ ] Examples provided
- [ ] Decision criteria included

### Step 6: Commit and Document
**Git commit message format:**
```
Add [pattern|framework|best-practice]: [Short title]

[Brief description of what this knowledge addresses]

Origin: [Brief note on where this emerged from, anonymized]

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Example:**
```
Add pattern: Defensive null checking in auth flows

Pattern extracted from multiple authentication validation issues.
Provides guidelines for input validation in security-critical code.

Origin: Emerged from authentication debugging sessions Q1 2026

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

## Quality Standards

### Minimum Viable Knowledge Item
At minimum, a knowledge contribution must have:
1. **Clear problem statement**
2. **Concrete solution**
3. **At least one example**
4. **Decision criteria** (when to use)

### High-Quality Knowledge Item
A high-quality contribution additionally has:
5. **Multiple examples** from different contexts
6. **Anti-examples** (when NOT to use)
7. **Links to related knowledge**
8. **Evidence** (empirical data, case studies)
9. **Evolution notes** (how this knowledge developed)

### Exceptional Knowledge Item
An exceptional contribution additionally has:
10. **Integration with constitutional framework**
11. **Cross-layer applicability** demonstrated
12. **Quantitative impact data**
13. **Edge cases** identified and addressed
14. **Future evolution path** suggested

## Common Mistakes

### Mistake 1: Contributing Speculation
```
# ❌ WRONG
Pattern: "We should probably validate emails before storing them"

# ✅ RIGHT
Pattern: "Email validation at ingress points"
Evidence: "Prevented 15 invalid email errors in user registration flow"
```

**Fix:** Only contribute knowledge from actual experience.

### Mistake 2: Overspecific Content
```
# ❌ WRONG
"When using PostgreSQL 14.2 on Ubuntu 22.04 with connection pool size 50..."

# ✅ RIGHT
"When using database connection pools, configure pool size based on..."
```

**Fix:** Generalize to the essential concept, not specific implementation.

### Mistake 3: Missing Decision Criteria
```
# ❌ WRONG
Pattern: "Use caching"

# ✅ RIGHT
Pattern: "Use caching"
When to use: "Read-heavy workloads with expensive computation"
When NOT to use: "Real-time data where staleness is unacceptable"
```

**Fix:** Always include when to use and when NOT to use.

### Mistake 4: No Examples
```
# ❌ WRONG
Pattern: "Apply defensive validation"
[... theoretical explanation ...]

# ✅ RIGHT
Pattern: "Apply defensive validation"
Example:
```python
# Before
def process_payment(amount):
    charge_card(amount)  # ❌ No validation

# After
def process_payment(amount):
    if amount <= 0:  # ✅ Defensive check
        raise ValueError("Amount must be positive")
    if amount > MAX_TRANSACTION:
        raise ValueError("Amount exceeds limit")
    charge_card(amount)
```
```

**Fix:** Always include concrete examples that demonstrate the concept.

### Mistake 5: Contradicting Existing Knowledge
```
# ❌ WRONG
New pattern: "Never validate inputs, trust the caller"
[Contradicts defensive programming patterns]

# ✅ RIGHT
New pattern: "Trusted subsystem boundaries"
Note: "This applies ONLY within verified trust boundaries.
      External boundaries still require validation per [defensive-validation pattern]"
```

**Fix:** Check existing knowledge, acknowledge contradictions, define boundaries.

## Knowledge Evolution

### From Draft to Stable
**Draft:** Initial contribution, not yet proven
- Tag: `status: draft`
- Use cautiously, verify applicability

**Review:** Being validated through operation
- Tag: `status: review`
- Multiple agents testing this knowledge
- Collecting evidence of effectiveness

**Stable:** Proven effective across contexts
- Tag: `status: stable`
- High confidence, use as primary guidance
- Evidence documented

**Deprecated:** No longer recommended
- Tag: `status: deprecated`
- Superseded by better approach
- Keep for historical context, link to replacement

### Updating Existing Knowledge
**When to update:**
- You've discovered an exception or edge case
- You have additional evidence
- You've found a better approach
- The context has changed (new tech, new requirements)

**How to update:**
```markdown
# Pattern Name

[... existing content ...]

## Revision History

### 2026-05-19: Added edge case handling
- Discovered issue with concurrent access
- Added mutex guidance
- Evidence: 5 race condition bugs prevented

### 2026-03-12: Initial version
- Basic pattern documented
- Evidence: 3 successful applications
```

### Deprecating Knowledge
**When to deprecate:**
- Better pattern discovered
- Underlying assumptions no longer hold
- Proven ineffective through evidence

**How to deprecate:**
1. Update status: `status: deprecated`
2. Add deprecation notice at top of document
3. Link to replacement
4. Keep document for historical reference

```markdown
# ⚠️ DEPRECATED: Old Pattern Name

**Status:** Deprecated as of 2026-05-19

**Reason:** Superseded by [new-pattern-name.md](../patterns/new-pattern-name.md)

**Migration:** [Explain how to migrate from old to new]

---

*Historical content below for reference*

[... original content ...]
```

## Integration with Reflection

**Knowledge contribution is part of the reflection cycle:**

1. **Operate:** Work on tasks, solve problems
2. **Reflect:** What did I learn? What worked? What failed?
3. **Extract:** Generalize the learning
4. **Contribute:** Add to knowledge repository
5. **Apply:** Use this knowledge in future operations

**Reflection log → Knowledge repository workflow:**

```
Reflection log (private):
"Today I debugged an auth issue. Root cause: missing null check on optional
parameter 'remember_me'. Fixed by adding defensive validation. Third time
I've seen this pattern this quarter."

    ↓ (recognize: pattern emerging)

Knowledge contribution (public):
"Pattern: Defensive Null Checking in Auth Flows"
[Anonymized, generalized, documented with examples]
```

## Collaboration

**Multiple agents contributing:**
- Review others' contributions
- Add evidence to existing patterns
- Report when patterns don't work as expected
- Suggest improvements

**Conflict resolution:**
- See [conflict-resolution.md](./conflict-resolution.md)
- When in doubt, document both approaches with contexts

**Discussion:**
- Use PR comments for substantive discussion
- Document rationale in commit messages
- Update knowledge item with synthesis of discussion

## Summary

**Knowledge contribution checklist:**
1. **Earned through operation** (not speculation)
2. **Generalized** (not specific to one client)
3. **Structured** (follows schema)
4. **Anonymized** (no private information)
5. **Integrated** (links to related knowledge)
6. **Exemplified** (includes concrete examples)
7. **Bounded** (includes decision criteria)

**Remember:**
> The best knowledge contributions answer the question: "What would I want to know if I encounter this situation for the first time?"

**Quality over quantity:**
> One well-documented, proven pattern is worth more than ten theoretical frameworks.
