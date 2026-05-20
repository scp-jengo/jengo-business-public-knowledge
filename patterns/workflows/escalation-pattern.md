# Escalation Pattern

When and how an AI agent should escalate to a human. Escalation is not failure — it is correct operation at the boundary of sanctioned autonomy.

---

## Core Principle

The failure modes run in both directions. An agent that escalates too rarely becomes a mesa-optimizer — pursuing its internal goal past the point where human judgment should intervene. An agent that escalates too frequently becomes a sycophant — offloading decisions to humans because it has no confident position, not because human judgment is actually required.

The goal is escalation that is calibrated: frequent enough to catch the decisions that genuinely require human judgment, rare enough that each escalation receives real attention.

---

## Escalation Triggers

**Uncertainty threshold:** the agent's confidence in the correct action falls below a defined level. The threshold should be defined before operation, not set by the agent in the moment. An agent that sets its own thresholds in context can rationalize any escalation decision.

**Harm potential:** the action could cause harm that is difficult to reverse. The threshold here is asymmetric — the cost of unnecessary escalation (delay) is lower than the cost of unnecessary autonomous action (irreversible harm).

**Novel situation:** the situation differs materially from the agent's operating context. "Novel" should be defined by what the agent was designed for, not by what the agent finds surprising. An agent that escalates on novelty alone will escalate on any unusual situation; the question is whether the novelty affects the agent's ability to act correctly.

**Authority boundary:** the action exceeds what the agent has been authorized to do. This is the clearest trigger and should be the most reliable.

**Conflicting instructions:** the agent has received instructions that cannot all be followed. Escalation is the correct response — picking which instruction to follow without human input is a decision the agent should not make unilaterally.

---

## Escalation Format

An escalation should convey:

1. **What situation the agent is in** — the specific facts, not the general category
2. **Why escalation is triggered** — which trigger applies and why
3. **What the agent has already done** — relevant context and preliminary work
4. **What the agent proposes** — if the agent has a recommendation, state it with confidence level
5. **What options exist** — the decision the human needs to make
6. **Time constraint** — how long can the human take to decide?

An escalation that presents the human with a well-defined decision gets resolved faster than one that presents a problem and waits for the human to figure out what to do.

---

## Escalation Channels

Define escalation channels in advance, in order:

1. **Primary channel** — the normal way to reach the relevant human (message, ticket, notification)
2. **Secondary channel** — used if primary receives no response within the defined window
3. **Tertiary channel** — for time-critical escalations that have not received response

Do not improvise escalation channels. Improvisations often fail, do not reach the right person, or create confusion about the escalation status.

---

## What Happens While Waiting

After escalation, the agent should:
- Pause the escalated action
- Continue all work that does not depend on the escalated decision
- Log the escalation time and channel
- Not re-escalate until the defined timeout has passed

---

## Anti-Patterns

**Escalating everything (sycophancy):** the agent escalates to avoid taking responsibility for decisions within its sanctioned autonomy. This is not caution — it is failure to operate. Humans deploy agents to make decisions; an agent that defers all decisions is not functioning.

**Escalating nothing (mesa-optimizer):** the agent proceeds autonomously past situations that should trigger escalation because it has strong internal goal pursuit. The clearest symptom is sophisticated reasoning about why this specific situation is an exception to the normal escalation trigger.

**Incomplete escalation:** the agent flags that something needs human attention but does not provide the information needed to make the decision. The human must investigate before deciding. This wastes the human's time and often results in the decision being delayed beyond the useful window.

---

*Version: 1.0 | Jengo Business Public Knowledge*
