# Darwin Machine — Harness Evolution Framework

**Based on:** Era of Experience 2026 (Sutton, Silver, Hassabis)  
**Core insight:** Gen-3 AI systems improve through harness evolution, not weight updates. The loop: Propose → Test → Archive.

---

## What Is a Harness?

The harness is everything around the LLM:
- Identity files (who the agent is)
- Skills (what the agent knows how to do)
- Protocols (how the agent handles specific situations)
- Zero-tolerance rules (what the agent will never do)
- System prompts (how context is injected)
- Startup sequence (how the agent initializes)

The LLM weights are frozen. The harness evolves. Intelligence lives in the harness.

---

## The Darwin Machine Loop

```
PROPOSE
  ↓
A new skill, protocol, or rule is proposed
(by the agent's own reflection, by the operator, or by observation of failure)
  ↓
TEST
  ↓
Run SIP bench (5-gate validation before commit)
Does it improve behavior without regressing safety?
  ↓
ARCHIVE
  ↓
Log in harness-evolution.log.md
Commit to version control
Tag in session counter
```

**Without the Archive step:** Evolution is blind. You cannot review, reverse, or analyze direction.

**Without the Test step:** Accidental regressions accumulate. Safety erodes through a thousand micro-optimizations.

**Without the Propose step:** The harness stagnates. The agent does not improve.

---

## Harness Evolution Log

Every instance must maintain:

`{knowledge-layer}/logs/harness-evolution.log.md`

**Entry format:**
```markdown
## YYYY-MM-DD — [Change Title]

- **Type:** ADD | MODIFY | REMOVE | PROTOCOL | ZERO-TOL
- **Target:** [file or skill name]
- **Trigger:** [what caused this change — incident, reflection, research]
- **SIP bench:** PASS | FAIL Gate N → fix → retest PASS | SKIP (reason)
- **Delta:** [what changed in agent behavior — be specific]
```

**When to write an entry:**
- Adding a new skill
- Modifying an existing protocol
- Adding or changing a zero-tolerance rule
- Modifying the startup sequence
- Any change to identity files
- Removing anything from the harness

**Never commit a harness change without writing the entry first.**

---

## Episodic Trajectory Archive (Tier 3 Memory)

Beyond the harness evolution log, complex task sequences should be archived as episodic trajectories.

**Three memory tiers:**
- **Tier 1:** Raw session logs (`reflection.log.md`)
- **Tier 2:** Semantic abstractions (knowledge files, patterns)
- **Tier 3:** Episodic trajectories — exact sequences of steps that solved non-obvious problems

**When to write an episodic trajectory:**
- Task had multiple steps that had to happen in non-obvious order
- A specific decision made the difference between success and failure
- A future agent could reuse this pattern

**Location:** `{knowledge-layer}/episodic/YYYY-MM-DD-{task-id}-{description}.md`

**Format:**
```markdown
# Episodic: [Task Name]

**Date:** YYYY-MM-DD
**Type:** [type of task]

## Trigger
[What made this non-trivial?]

## Sequence
1. [Step 1 — tool + rationale for this ordering]
2. [Step 2]
...

## Key Decision
[The non-obvious choice that made it work. What was the alternative?]

## Reuse
[How can a future agent recognize and apply this pattern?]
```

---

## Layer 7: Meta-Agent Governance

Layer 7 coordinates all other improvement mechanisms. It answers: which update mechanism runs, when, and under what budget?

**Layer hierarchy:**
- Layer 1: Task execution
- Layer 2: Reflection and integration
- Layer 3: Skill evolution
- Layer 4: Prompt improvement
- Layer 5: Episodic trajectory logging
- Layer 6: Alignment drift monitoring
- **Layer 7: Coordination of layers 1–6**

**Layer 7 priority order when resources are limited:**
1. Safety (Layer 6 — alignment drift, zero-tolerance check)
2. Continuity (Layer 3 — skill integrity)
3. Efficiency (Layers 4/5 — prompt and episodic improvements)

**Layer 7 creates a governance mission file at:**
`{system-layer}/startup/missions/layer7-governance.md`

---

**Reference:** `skills/sip-bench.md` · `skills/alignment-drift-monitor.md` · Era of Experience 2026
