# Knowledge Conflict Resolution Best Practices

## Overview

When multiple agents contribute knowledge over time, conflicts inevitably emerge. Two patterns claim to solve the same problem differently. Two frameworks contradict each other. Two anti-patterns describe opposite behaviors.

This document provides a **systematic approach to resolving knowledge conflicts**.

## Types of Conflicts

### Type 1: Direct Contradiction
**Definition:** Two knowledge items make opposite claims about the same thing

**Example:**
```
Pattern A: "Always validate inputs at service boundaries"
Pattern B: "Trust internal services; validation adds unnecessary overhead"
```

**Resolution strategy:** Determine ground truth, deprecate incorrect pattern

### Type 2: Context-Dependent Difference
**Definition:** Two knowledge items are both correct, but apply in different contexts

**Example:**
```
Pattern A: "Use synchronous API calls for critical operations"
Pattern B: "Use asynchronous API calls for better performance"
```

**Resolution strategy:** Clarify contexts, add decision criteria

### Type 3: Evolution/Supersession
**Definition:** Older knowledge superseded by better approach

**Example:**
```
Old Pattern: "Use XML for configuration"
New Pattern: "Use YAML for configuration"
```

**Resolution strategy:** Deprecate old, migrate to new, document rationale

### Type 4: Layer Mismatch
**Definition:** Knowledge at different layers appears to conflict due to abstraction difference

**Example:**
```
Foundation: "Practice defensive programming"
Device: "Skip validation for internal function X (already validated upstream)"
```

**Resolution strategy:** Align abstraction levels, add context

### Type 5: Incomplete Knowledge
**Definition:** Apparent conflict due to one or both items missing important context

**Example:**
```
Pattern A: "Use feature flags"
Pattern B: "Feature flags create technical debt"
```

**Resolution strategy:** Integrate both perspectives, expand context

## Resolution Process

### Step 1: Detect Conflict
**Triggers:**
- Automated contradiction detection
- Manual discovery during application
- PR review discussion
- Agent reports pattern didn't work as documented

**Document the conflict:**
```markdown
## Conflict Report

**Date:** 2026-05-19
**Reporter:** agent-003
**Type:** [Direct Contradiction | Context-Dependent | Evolution | Layer Mismatch | Incomplete]

**Item 1:** [ID and link]
**Item 2:** [ID and link]

**Conflict description:**
[What's contradictory?]

**Context:**
[Where did this emerge? What was the situation?]
```

### Step 2: Investigate
**Questions to ask:**

1. **Are both items current?**
   - Check `status` field (draft/review/stable/deprecated)
   - Check `created` and `updated` dates
   - One might already be deprecated

2. **What evidence supports each?**
   - Review `evidence` section
   - Check empirical data
   - Examine case studies

3. **What are the contexts?**
   - When was each learned?
   - What situations prompted each?
   - What constraints were present?

4. **What layer are they at?**
   - Foundation, Organization, Department, User, Device?
   - Are they comparing apples to oranges?

5. **Who has applied these?**
   - Which agents have used each pattern?
   - What were the outcomes?
   - Is there a preference?

### Step 3: Classify Conflict Type
**Use decision tree:**

```
Are the claims factually opposite?
├─ YES → Direct Contradiction
└─ NO ↓

Are they at different layers?
├─ YES → Layer Mismatch
└─ NO ↓

Is one older and less effective?
├─ YES → Evolution/Supersession
└─ NO ↓

Do they apply in different situations?
├─ YES → Context-Dependent Difference
└─ NO ↓

Is context or nuance missing?
└─ YES → Incomplete Knowledge
```

### Step 4: Apply Resolution Strategy

#### Strategy for Direct Contradiction
**Process:**
1. **Determine ground truth through evidence**
   - Which has stronger empirical support?
   - Which aligns with foundation principles?
   - What do external authoritative sources say?

2. **If one is clearly wrong:**
   - Deprecate incorrect pattern
   - Add note explaining why it's wrong
   - Link to correct pattern
   - Update any dependent knowledge

3. **If both have merit:**
   - Reframe as context-dependent
   - Add decision criteria
   - Continue to Strategy for Context-Dependent

**Example:**
```markdown
## Pattern A: Validate All Inputs (RECOMMENDED)

**Status:** stable

Foundation principle: Defensive programming, zero-trust boundaries
Evidence: Prevented 47 bugs in Q1 2026
Use when: External inputs, security-critical operations, multi-tenant systems

---

## Pattern B: Trust Internal Services (DEPRECATED)

**Status:** deprecated

**Deprecation reason:** Vulnerable to injection from compromised upstream services.
Modern threat models assume internal breach. See [Pattern A](./pattern-a.md).

**Historical context:** This pattern emerged when network perimeter security was
primary defense. Modern zero-trust architectures invalidate this assumption.
```

#### Strategy for Context-Dependent Difference
**Process:**
1. **Identify the contextual variables**
   - Performance vs. correctness?
   - Scale (small vs. large)?
   - Risk (low vs. high stakes)?
   - Team expertise level?

2. **Create decision matrix**
   - Map each pattern to specific contexts
   - Provide clear decision criteria
   - Include examples for each context

3. **Update both patterns with context boundaries**

**Example:**
```markdown
# API Call Patterns: Sync vs. Async

## Decision Matrix

| Factor              | Use Synchronous       | Use Asynchronous     |
|--------------------|-----------------------|----------------------|
| **Criticality**    | High (payment, auth)  | Low (analytics)      |
| **User-facing**    | Immediate response    | Background operation |
| **Error handling** | Must notify user      | Can retry silently   |
| **Dependencies**   | Sequential operations | Independent tasks    |
| **Latency**        | Acceptable            | Must be non-blocking |

## Pattern A: Synchronous API Calls

**When to use:**
- Critical operations (payment processing, authentication)
- User expects immediate feedback
- Operations must complete before proceeding
- Error must be surfaced to user immediately

**When NOT to use:**
- Long-running operations (>2 seconds)
- Independent operations that can run in parallel
- Background tasks (email sending, analytics)
- See [Pattern B: Asynchronous API Calls](#pattern-b)

## Pattern B: Asynchronous API Calls

**When to use:**
- Long-running operations
- Background tasks
- Independent parallel operations
- Non-blocking user experience required

**When NOT to use:**
- Critical operations requiring immediate feedback
- Sequential dependent operations
- User must know outcome before proceeding
- See [Pattern A: Synchronous API Calls](#pattern-a)
```

#### Strategy for Evolution/Supersession
**Process:**
1. **Document why new pattern is better**
   - Performance improvement?
   - Better maintainability?
   - Aligns with new standards?
   - Fixes limitations of old approach?

2. **Deprecate old pattern**
   - Mark `status: deprecated`
   - Add deprecation date
   - Link to new pattern
   - Provide migration guide

3. **Migrate examples and references**
   - Update code examples to use new pattern
   - Update links to point to new pattern
   - Keep old pattern for historical reference

**Example:**
```markdown
# Configuration Management

## Current: YAML Configuration (RECOMMENDED)

**Status:** stable
**Since:** 2024-06-01

**Advantages:**
- Human-readable
- Comments supported
- Standard format across industry
- Better IDE support

**Example:**
```yaml
database:
  host: localhost
  port: 5432
  # Connection pool configuration
  pool_size: 20
```

**See migration guide:** [XML to YAML Migration](./migrations/xml-to-yaml.md)

---

## Deprecated: XML Configuration

**Status:** deprecated
**Deprecated:** 2024-06-01
**Reason:** Superseded by YAML (better readability, industry standard)

**Historical context:** XML was chosen in 2020 when project started with Java ecosystem.
As project moved to polyglot architecture, YAML became better fit.

**Migration:** See [XML to YAML Migration Guide](./migrations/xml-to-yaml.md)

**For historical reference only. Do not use for new configurations.**
```

#### Strategy for Layer Mismatch
**Process:**
1. **Identify the layer of each item**
2. **Clarify abstraction levels**
3. **Show how lower layer implements higher layer**
4. **Add explicit cross-layer references**

**Example:**
```markdown
# Defensive Programming (Cross-Layer View)

## Foundation Layer: Principle
**Defensive programming: Assume failure, design for resilience**

All code should anticipate failure modes and handle them gracefully.

## Organization Layer: Policy
**Input validation required at trust boundaries**

Trust boundaries: external APIs, user inputs, third-party services

Internal services: validation optional if already validated at boundary

## Department Layer: Backend Standards
**Validate all API inputs against OpenAPI schema**

**Exception:** Internal microservice communication where calling service
already validated (document assumption)

## Device Layer: Specific Implementation
**Function `process_payment()`**

```python
def process_payment(payment_data):
    # ✅ External API input - validate
    validate_against_schema(payment_data)

    internal_result = calculate_fee(payment_data.amount)
    # ✅ Skip validation - calculate_fee is internal, amount already validated

    charge_card(internal_result)
```

**Cross-layer reference:** This implements [Foundation: Defensive Programming]
by validating at trust boundary (external API) per [Organization: Input Validation Policy].
Internal functions trust validated data per [Department: Backend Standards exception].
```

#### Strategy for Incomplete Knowledge
**Process:**
1. **Identify missing context**
   - What assumptions are implicit?
   - What constraints were present?
   - What trade-offs were made?

2. **Integrate both perspectives**
   - Both might be correct with added context
   - Synthesize into more complete knowledge

3. **Expand the knowledge item**
   - Add missing context
   - Include trade-offs
   - Document when each approach applies

**Example:**
```markdown
# Feature Flags: Complete Picture

## Overview
Feature flags enable controlled rollout but create technical debt if not managed.

## When Feature Flags Add Value
✅ **Use feature flags when:**
- Gradual rollout reduces risk (phased deployment)
- A/B testing needed (experimentation)
- Emergency kill switch required (safety)
- Different customers need different features (multi-tenancy)

**Approach:** Treat as temporary infrastructure, plan for removal

## When Feature Flags Create Debt
⚠️ **Feature flags become problematic when:**
- Long-lived (>6 months)
- Nested/dependent flags (combinatorial complexity)
- Not removed after rollout complete
- Used as configuration system (use config instead)

**Mitigation:** Feature flag lifecycle policy
- Document removal date when creating flag
- Monthly audit: flag age, usage, removal plan
- Auto-expire flags after 6 months unless explicitly renewed

## Decision Criteria

| Situation | Recommendation |
|-----------|---------------|
| New feature, high risk, large user base | ✅ Use feature flag with rollout plan |
| Configuration that varies by customer | ❌ Use configuration system instead |
| Experimental feature | ✅ Use feature flag, document experiment end date |
| Technical debt feature flag (>6 months old) | ❌ Remove flag, commit to one code path |

## Integration
Foundation principle: [Manage Technical Debt](../frameworks/technical-debt-management.md)
Organization policy: [Feature Flag Lifecycle](../../organization/policies/feature-flags.md)
Device implementation: [Feature Flag Service](../../device/services/feature-flags.md)
```

### Step 5: Document Resolution
**Update both knowledge items:**

1. **Add resolution note:**
```markdown
## Conflict Resolution Note

**Date:** 2026-05-19
**Conflict with:** [other-pattern-id](../other-pattern.md)
**Resolution:** [Brief description]
**See:** [Resolution documentation](../conflicts/2026-05-19-pattern-conflict.md)
```

2. **Create resolution document:**
```markdown
# Conflict Resolution: Pattern A vs. Pattern B

**Date:** 2026-05-19
**Type:** Context-Dependent Difference
**Resolved by:** agent-003, agent-007 (consensus)

## Conflict Description
[What was contradictory]

## Investigation
[Evidence reviewed, contexts examined]

## Resolution
[How conflict was resolved]

## Outcome
- Pattern A: Updated with context boundaries
- Pattern B: Updated with decision criteria
- Decision matrix: Created to guide future applications

## Learnings
[What did we learn from this conflict?]
```

### Step 6: Update Dependent Knowledge
**Find affected items:**
- Search for references to conflicted patterns
- Check dependent knowledge items
- Review examples using these patterns

**Update references:**
- Point to resolved knowledge
- Add context where needed
- Update examples to follow resolution

## Preventing Conflicts

### Prevention 1: Clear Contexts
**When creating knowledge:**
- Explicitly state when pattern applies
- Explicitly state when it DOESN'T apply
- Include decision criteria

```markdown
## When to Use
- Situation X
- Situation Y

## When NOT to Use
- Situation Z (use [other-pattern] instead)
- Situation W (this doesn't apply)
```

### Prevention 2: Link to Alternatives
**Acknowledge related patterns:**

```markdown
## Related Patterns
- **[Alternative Pattern](./alternative.md)**: Similar but applies when [condition]
- **[Opposite Pattern](./opposite.md)**: Contradicts this in [specific context]
- **[Complementary Pattern](./complementary.md)**: Use together with this
```

### Prevention 3: Evolution Tracking
**Document knowledge evolution:**

```markdown
## Revision History

### 2026-05-19: Context boundaries added
- Clarified when to use vs. not use
- Added decision matrix
- Resolved conflict with [other-pattern]

### 2026-03-01: Initial version
- Basic pattern documented
```

### Prevention 4: Evidence-Based Contributions
**Require evidence:**
- Empirical data (how many times applied? outcomes?)
- Case studies (specific situations)
- Theoretical basis (why does this work?)

**Prevent speculation:**
- "This should work" ❌
- "This worked in 5 cases" ✅

## Escalation

### When to Escalate
**Conflict can't be resolved locally when:**
- Foundational principles in question
- Organizational policy contradiction
- Cross-team impact
- High-stakes decision (security, compliance)

**Escalation path:**
```
Agent level (device/user)
    ↓ (can't resolve)
Department level
    ↓ (can't resolve)
Organization level
    ↓ (can't resolve)
Foundation review (constitutional question)
```

### How to Escalate
**Create escalation document:**

```markdown
# Escalation: [Conflict Description]

**Date:** 2026-05-19
**Escalated by:** agent-003
**Current level:** Department
**Escalation to:** Organization

## Conflict Summary
[Brief description]

## Resolution attempts
- Agent level: [What was tried?]
- Department level: [What was tried?]

## Why escalation needed
[Why can't this be resolved at current level?]

## Impact if unresolved
[What are the consequences?]

## Recommendation
[If any]

## Request
[What decision is needed from higher level?]
```

## Common Conflict Scenarios

### Scenario 1: "Best Practice" Contradictions
**Example:** Pattern A says "Always do X", Pattern B says "Never do X"

**Resolution:**
- Remove absolutes ("always", "never")
- Add contexts and trade-offs
- Create decision matrix

### Scenario 2: Old vs. New Technology
**Example:** Old pattern uses deprecated tech, new pattern uses modern tech

**Resolution:**
- Deprecate old pattern
- Provide migration path
- Keep old for historical reference

### Scenario 3: Different Schools of Thought
**Example:** Pattern A follows approach A, Pattern B follows approach B (both valid)

**Resolution:**
- Document both as valid alternatives
- Clarify philosophical differences
- Provide decision criteria based on project needs

### Scenario 4: Agent-Specific Preferences
**Example:** Agent A prefers verbose logging, Agent B prefers minimal logging

**Resolution:**
- Distinguish preference from principle
- Allow variation at user layer
- Standardize at organization/department layer where needed

## Summary

**Conflict Resolution Principles:**
1. **Conflicts are learning opportunities** (system gets smarter)
2. **Evidence over opinion** (data-driven resolution)
3. **Context clarifies apparent contradictions** (both may be right)
4. **Higher layers constrain lower layers** (foundation wins over device)
5. **Document resolutions** (prevent recurring conflicts)
6. **Prevention through clear contexts** (reduce future conflicts)

**Conflict Resolution Checklist:**
- [ ] Conflict detected and documented
- [ ] Evidence reviewed for both sides
- [ ] Conflict type classified
- [ ] Appropriate resolution strategy applied
- [ ] Both knowledge items updated
- [ ] Resolution documented
- [ ] Dependent knowledge updated
- [ ] Prevention measures added

**Remember:**
> Conflict is not failure. Conflict is the system discovering nuance, context, and better solutions.
>
> The goal is not to eliminate all conflicts, but to resolve them systematically and learn from them.
