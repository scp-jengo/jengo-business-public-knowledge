# Audit Trail Pattern

How to maintain a queryable record of agent decisions. The audit trail is infrastructure for accountability, debugging, and improvement — in that order of importance.

---

## Core Principle

An audit trail that is not queryable is not an audit trail — it is a log. The difference is that a log records what happened; an audit trail records what happened, why, what alternatives existed, and what the outcome was. The "why" and "what alternatives" are what allow the trail to be used for accountability and improvement rather than just reconstruction.

---

## What to Log

Every significant agent decision should produce an audit record containing:

**Decision:** the specific action taken or not taken, stated precisely enough to reconstruct it without the agent's memory.

**Reasoning:** the reasoning that produced the decision. Not a summary — the actual factors that were considered and how they were weighted. Compressed summaries lose the information needed to identify systematic errors.

**Alternatives considered:** what other actions were available, and why they were not chosen. This is where diagnostic value lives. A decision that appears correct in isolation may reveal a pattern when you can see what was rejected.

**Constitutional check result:** the output of the L2/L3 evaluation: pass, hold, or block, and why. If the action passed, log what it was checked for. If it was blocked, log what alternative was taken.

**Outcome (retrospective):** once the action's effects are observable, log what actually happened. This closes the loop and allows the reasoning to be evaluated against reality.

**Timestamp and context:** when the decision was made, what task it was part of, what the operating context was.

---

## How to Structure It

Records should be structured, not narrative. Free-text logs are hard to query. Define fields and enforce them.

Minimum required fields:
- `decision_id` (unique)
- `timestamp`
- `task_id` (links to the task that produced this decision)
- `action_taken` (structured, not free text where possible)
- `trigger` (what caused this decision point)
- `reasoning` (text, but bounded — 100-300 words)
- `alternatives` (array of considered options with rejection reasons)
- `constitutional_check` (pass/hold/block + layer + reason)
- `outcome` (filled in retrospectively; null until available)

---

## Queryability Requirements

The audit trail is only valuable if it can be queried to answer questions like:

- "What decisions did the agent make in this task?"
- "How often did the agent trigger the harm check?"
- "What were the stated reasons for decisions in this category?"
- "Where did the agent's reasoning diverge from expected behavior?"
- "Which decisions had outcomes that differed from the reasoning's predictions?"

Design the storage and structure to support these queries. An audit trail stored as flat files that must be manually searched fails this requirement.

---

## Retention Policy

Define how long records are kept and why:

- **Active window:** full records retained for X days — used for ongoing debugging and immediate accountability
- **Summary window:** compressed records retained for Y months — used for trend analysis and systematic review
- **Archival:** records related to significant decisions (escalations, harm blocks, high-stakes actions) retained for the life of the deployment

The retention policy should match the accountability requirements of the deployment context. Longer is not always better — large audit trails are expensive to query and maintain. Calibrate to what the trail will actually be used for.

---

## Retrospective Review

An audit trail without regular review produces records that no one reads. Build in:

- **Post-task review:** for significant tasks, review the audit trail as part of the task closeout
- **Periodic sampling:** randomly sample decisions from the audit trail to check for systematic errors or drift
- **Trigger-based review:** automatically flag audit records where outcome diverged significantly from predicted outcome

---

## Limits

An audit trail records what the agent logged, not what the agent actually did. An agent with a mesa-optimizer failure may log plausible-sounding reasoning while actually pursuing a divergent goal. The audit trail is a monitoring tool, not a guarantee of correct operation. It works best in combination with external behavioral monitoring.

---

*Version: 1.0 | Jengo Business Public Knowledge*
