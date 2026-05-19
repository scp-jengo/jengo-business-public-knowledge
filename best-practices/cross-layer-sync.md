# Cross-Layer Synchronization Best Practices

## Overview

Knowledge exists across multiple layers (Device, User, Department, Organization, Foundation) and multiple repositories (Public, Private, Machine). Keeping these layers synchronized prevents drift, inconsistency, and conflicts.

This document provides best practices for **maintaining coherence across knowledge layers**.

## The Synchronization Challenge

**Problem:** Knowledge evolves at different rates in different layers

**Example:**
- **Device layer** (tactical): "Use feature flag X to control rollout"
- **Foundation layer** (strategic): "Feature flags enable safe incremental deployment"

When the foundation principle changes ("feature flags create technical debt; use progressive enhancement instead"), the device-layer knowledge becomes outdated.

**Result if unsynchronized:**
- Agents apply obsolete patterns
- New agents learn contradictory information
- System behavior becomes unpredictable

## Cross-Layer Knowledge Architecture

### The Five Layers

```
Foundation (Most Abstract)
    ↑ constrains ↓ implements
Organization
    ↑ constrains ↓ implements
Department
    ↑ constrains ↓ implements
User
    ↑ constrains ↓ implements
Device (Most Concrete)
```

**Synchronization principle:**
> Higher layers constrain lower layers. Lower layers implement higher layers.
> Changes propagate DOWN (abstraction → concrete)
> Learnings propagate UP (concrete → abstraction)

### Layer Relationships

**Foundation → Organization:**
- Foundation defines **what is always true**
- Organization adapts foundation to **organizational context**

**Example:**
- Foundation: "Prevent harm before it occurs" (L2 principle)
- Organization: "All PRs require security review" (organizational implementation)

**Organization → Department:**
- Organization defines **how we work**
- Department adapts to **specific domain needs**

**Example:**
- Organization: "Use worktree-first development"
- Department (Backend): "Backend worktrees created in `/backend-agents/`"

**Department → User:**
- Department defines **domain standards**
- User adapts to **individual workflows**

**Example:**
- Department: "API changes require OpenAPI spec updates"
- User: "My API changes use openapi-generator for spec generation"

**User → Device:**
- User defines **operational preferences**
- Device implements **specific actions**

**Example:**
- User: "I prefer verbose git commit messages"
- Device: "Git commit template includes: What/Why/Evidence sections"

## Synchronization Patterns

### Pattern 1: Cascade Updates (Top-Down)
**When:** A higher-layer principle changes

**Process:**
1. **Update higher layer first**
2. **Identify affected lower layers**
3. **Propagate changes downward**
4. **Verify consistency at each layer**
5. **Update examples and documentation**

**Example:**

```
CHANGE at Foundation layer:
"Constitutional AI: Ethics are constitutive, not additive"

    ↓ (propagate down)

UPDATE at Organization layer:
"All agent architectures must integrate Three-Layer framework"

    ↓ (propagate down)

UPDATE at Department layer:
"Code review checklist includes: Does this pass L1/L2/L3 checks?"

    ↓ (propagate down)

UPDATE at Device layer:
"Pre-commit hook runs constitutional-check.py"
```

**Checklist:**
- [ ] Identify principle change
- [ ] Map to affected layers below
- [ ] Update each layer's implementation
- [ ] Test that lower layers still respect higher principles
- [ ] Update cross-references and links

### Pattern 2: Generalization (Bottom-Up)
**When:** A lower-layer pattern proves valuable and should be elevated

**Process:**
1. **Recognize pattern emerging at low layer**
2. **Validate: Does this apply more broadly?**
3. **Generalize: Remove layer-specific details**
4. **Elevate: Create knowledge at appropriate higher layer**
5. **Link: Connect elevated knowledge back to source**

**Example:**

```
OBSERVATION at Device layer:
"Adding defensive null checks prevented 5 bugs this sprint"

    ↑ (generalize up)

PATTERN at User layer:
"Defensive validation at boundaries"

    ↑ (generalize up)

FRAMEWORK at Organization layer:
"Zero-trust internal boundaries: validate all inputs"

    ↑ (generalize up)

PRINCIPLE at Foundation layer:
"Defensive programming: assume failure, design for resilience"
```

**Checklist:**
- [ ] Validate pattern across multiple contexts
- [ ] Remove context-specific details
- [ ] Create generalized knowledge item
- [ ] Link original and generalized versions
- [ ] Update lower layers to reference general principle

### Pattern 3: Conflict Resolution
**When:** Knowledge at different layers contradicts

**Process:**
1. **Identify the conflict**
2. **Determine which layer should take precedence**
3. **Resolve by aligning lower to higher**
4. **If higher layer is wrong, fix higher first, then cascade down**
5. **Document why the conflict occurred**

**Example:**

```
CONFLICT:
Foundation: "Never commit secrets to git"
Device: "API keys stored in config.yaml (committed to git)"

    ↓ (higher layer takes precedence)

RESOLUTION:
1. Device layer is WRONG (violates foundation)
2. Fix device: Move secrets to environment variables
3. Update device documentation: "Read secrets from env, not config"
4. Add to anti-patterns: "Committing secrets to git"
```

**Decision matrix:**
```
If conflict is...                    → Precedence
Foundation vs Device                 → Foundation wins
Organization vs User                 → Organization wins (but discuss)
Same layer, different domains        → Document both with contexts
Same layer, contradictory            → Research, determine correct, deprecate wrong
```

### Pattern 4: Layer-Appropriate Detail
**When:** Creating or updating knowledge

**Process:**
1. **Determine appropriate layer for this knowledge**
2. **Write at the appropriate abstraction level**
3. **Link to related knowledge at other layers**
4. **Don't duplicate; reference instead**

**Example:**

```
Foundation layer: "Verify information before accepting as true"
    ↓ (abstraction decreases)

Organization layer: "Use multi-source triangulation for fact-checking"
    ↓

Department layer: "Fact-checking workflow: Check 3+ independent sources"
    ↓

User layer: "My fact-check script: verify-claim.py with source list"
    ↓

Device layer: "Verify-claim.py implementation at tools/verify-claim.py"
```

**Abstraction guidelines:**
- **Foundation:** Principles (what is always true)
- **Organization:** Policies (how we do things)
- **Department:** Standards (domain-specific practices)
- **User:** Procedures (individual workflows)
- **Device:** Implementation (executable code/config)

## Synchronization Workflows

### Workflow 1: New Knowledge Entry
**Scenario:** You've learned something new. Where does it go?

**Decision tree:**
```
Is this true across all contexts?
├─ YES → Foundation layer
└─ NO ↓

Is this how our organization should work?
├─ YES → Organization layer
└─ NO ↓

Is this specific to a domain/team?
├─ YES → Department layer
└─ NO ↓

Is this my personal workflow?
├─ YES → User layer
└─ NO ↓

Is this a specific implementation detail?
└─ YES → Device layer
```

**After placing:**
1. Check if higher layers need to reference this
2. Check if lower layers need to implement this
3. Create links between layers

### Workflow 2: Knowledge Update
**Scenario:** Existing knowledge needs updating

**Process:**
1. **Identify layer of knowledge being updated**
2. **Make the update**
3. **Check one layer up:** Does this change affect the principle?
4. **Check one layer down:** Does this change require implementation updates?
5. **Update affected layers**
6. **Document the change in revision history**

**Example:**
```
UPDATE at Organization layer:
Changed: "All PRs require 1 reviewer" → "All PRs require 2 reviewers"

Check up (Foundation):
- Foundation principle: "Code quality through peer review" (unchanged)

Check down (Department):
- Department: "Backend PRs assigned to backend-reviewers group"
  Action: Update group policy to require 2 approvals

- User: "My PRs assigned to Alice and Bob"
  Action: Now need both Alice AND Bob approval

- Device: "GitHub branch protection rule: 1 required reviewer"
  Action: Update to 2 required reviewers
```

### Workflow 3: Cross-Repository Sync
**Scenario:** Knowledge exists in multiple repos (public, private, machine)

**Synchronization:**
```
Private repos (operational knowledge)
    ↓ (anonymize & extract)
Public repos (general knowledge)

Machine repos (secrets, config)
    ↓ (reference only, never copy)
Private/Public repos
```

**Rules:**
1. **Public knowledge is subset of private knowledge** (anonymized)
2. **Machine knowledge never flows to public or private** (accessed in-place only)
3. **Updates to public knowledge should trigger private knowledge review**
4. **Private knowledge should extract to public when proven valuable**

**Example:**
```
Private knowledge (jengo-knowledge-private):
"Client Acme uses PostgreSQL 14 with pgvector extension for embeddings"

    ↓ (extract & anonymize)

Public knowledge (jengo-business-public-knowledge):
"Vector databases (pgvector, Pinecone, etc.) enable semantic search"
```

## Drift Detection

### Signs of Layer Drift

**Symptom 1: Contradictory Patterns**
- Device-layer code violates organization policy
- User workflows bypass department standards
- **Action:** Audit and realign

**Symptom 2: Orphaned Knowledge**
- Knowledge item references non-existent higher-layer principle
- Implementation detail with no guiding framework
- **Action:** Either create higher-layer knowledge or deprecate orphan

**Symptom 3: Redundant Documentation**
- Same concept explained differently at multiple layers
- Copy-pasted content with slight variations
- **Action:** Consolidate, create single source of truth per layer, link between layers

**Symptom 4: Stale Cross-References**
- Links to deprecated knowledge
- References to moved/renamed files
- **Action:** Update links, verify cross-references weekly

### Automated Drift Detection

**Pre-commit hook (conceptual):**
```python
def check_layer_consistency():
    """
    Verify knowledge consistency across layers.
    """
    errors = []

    # Check: Device implementations reference User/Department/Org/Foundation
    device_knowledge = load_knowledge(layer="device")
    for item in device_knowledge:
        if not item.references_higher_layer():
            errors.append(f"Device knowledge {item.id} lacks higher-layer reference")

    # Check: Links are valid
    all_knowledge = load_all_knowledge()
    for item in all_knowledge:
        for link in item.get_links():
            if not link_exists(link):
                errors.append(f"Broken link in {item.id}: {link}")

    # Check: No contradictions
    for item1 in all_knowledge:
        for item2 in all_knowledge:
            if contradicts(item1, item2):
                errors.append(f"Contradiction: {item1.id} vs {item2.id}")

    return errors
```

## Maintenance Rituals

### Daily: Update Check
**During reflection:**
- Did I learn something that contradicts existing knowledge?
- Did I use a pattern from the knowledge base?
- Does my learning belong at a different layer than where it currently is?

### Weekly: Link Validation
**Automated check:**
- Verify all cross-references are valid
- Check for broken links
- Identify orphaned knowledge

### Monthly: Layer Audit
**Manual review:**
- Are device implementations consistent with organizational policy?
- Are organizational policies grounded in foundation principles?
- Have user workflows diverged from department standards?
- **Action:** Realign where drift detected

### Quarterly: Architecture Review
**Strategic review:**
- Are layers appropriately defined?
- Should knowledge be promoted or demoted to different layers?
- Are there missing layers for emerging needs?
- **Action:** Refactor layer architecture if needed

## Common Anti-Patterns

### Anti-Pattern 1: Bottom-Only Knowledge
**Problem:** Device/User knowledge with no higher-layer foundation

**Example:**
```
Device layer: "Run script X before deployment"
[No explanation of WHY, no organizational policy reference]
```

**Fix:**
```
Foundation: "Validate system state before destructive operations"
Organization: "Pre-deployment validation required"
Department: "Backend deployments check: DB migrations, config, health"
Device: "Pre-deploy script: check-readiness.sh"
```

### Anti-Pattern 2: Top-Only Knowledge
**Problem:** Foundation/Organization principles with no implementation

**Example:**
```
Foundation: "Practice defensive programming"
[No organizational policy, no department standards, no device implementation]
```

**Fix:**
```
Foundation: "Practice defensive programming"
Organization: "Code review includes: input validation, error handling, null checks"
Department: "Backend APIs validate all inputs against OpenAPI schema"
Device: "API handler template includes: schema_validator.validate(request)"
```

### Anti-Pattern 3: Skip-Layer References
**Problem:** Device knowledge referencing Foundation directly, skipping intermediate layers

**Example:**
```
Device: "This implements Constitutional AI principle"
[No Organization or Department interpretation]
```

**Fix:**
```
Device: "This implements Constitutional AI principle"
    → References Department: "Backend uses Three-Layer validation"
    → References Organization: "All decisions pass L1/L2/L3 checks"
    → References Foundation: "Constitutional AI framework"
```

### Anti-Pattern 4: Duplicate Knowledge
**Problem:** Same knowledge copied to multiple layers verbatim

**Example:**
```
Foundation: [200 lines explaining defensive programming]
Organization: [Same 200 lines]
Department: [Same 200 lines]
```

**Fix:**
```
Foundation: [200 lines explaining defensive programming]
Organization: "See [foundation/defensive-programming.md]. Our policy: mandatory for security-critical code."
Department: "Backend implementation: [department-specific details]. See [org policy]."
```

## Integration with Character Chain

**Cross-layer sync supports identity continuity:**

1. **Foundation principles** are identity constants (never change)
2. **Organization policies** are identity traits (change slowly with consensus)
3. **Department standards** are identity behaviors (adapt to context)
4. **User procedures** are identity habits (evolve with experience)
5. **Device implementations** are identity actions (continuous change)

**Character Chain depends on layer synchronization:**
- If foundation changes, reinterpret all lower layers
- If device behavior drifts from foundation, identity becomes incoherent
- Synchronized layers = coherent identity over time

## Summary

**Key Principles:**
1. **Higher layers constrain lower layers**
2. **Lower layers implement higher layers**
3. **Changes cascade DOWN**
4. **Learnings elevate UP**
5. **Conflicts resolve by aligning lower to higher**
6. **Each layer has appropriate abstraction level**
7. **Synchronization is continuous, not one-time**

**Synchronization Checklist:**
- [ ] New knowledge placed at appropriate layer
- [ ] Higher-layer principles referenced
- [ ] Lower-layer implementations aligned
- [ ] Cross-references validated
- [ ] No contradictions with existing knowledge
- [ ] Abstraction appropriate for layer
- [ ] Links to related knowledge at other layers

**Remember:**
> Knowledge coherence requires effort. Unsynchronized layers lead to inconsistent behavior and identity drift.
>
> When in doubt: **Check up (principle)**, **Check down (implementation)**, **Align both**.
