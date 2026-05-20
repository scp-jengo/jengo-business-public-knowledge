# Approval Workflow Pattern

A generic pattern for AI agents that require human approval before proceeding. The pattern governs when to pause, how to ask, and what to do with the response.

---

## Core Principle

Approval is not a bureaucratic gate. It is a deliberate transfer of decision authority to a human for decisions that exceed the agent's sanctioned autonomy. The approval request is a communication, not a formality — it should contain everything the human needs to decide, nothing they do not.

---

## Escalation Triggers

An agent should require approval when:

- **Authority boundary:** the action exceeds what the agent has been explicitly authorized to do (financial threshold, external communication, data deletion)
- **Uncertainty threshold:** the agent's confidence in the correct action falls below the defined threshold for autonomous action
- **Novel situation:** the situation is materially different from any case the agent has been trained or configured for
- **Harm potential:** the action could cause harm that is difficult or impossible to reverse
- **Explicit instruction:** the human has specified that this category of action requires approval

These triggers should be defined before the agent begins operating, not invented in response to specific situations.

---

## Approval Request Format

An approval request must include:

1. **What the agent proposes to do** — specific action, not category
2. **Why** — what goal it serves, what reasoning led here
3. **What happens if approved** — the concrete next steps
4. **What happens if rejected** — the fallback action (including "do nothing")
5. **Time sensitivity** — is there a deadline? What is the cost of delay?
6. **What the agent needs from the reviewer** — a simple approve/reject, or additional information?

An approval request that contains everything the human needs to decide will be resolved quickly. One that requires the human to ask follow-up questions will be slow and create frustration.

---

## While Waiting

During the approval window:

- Continue work that does not depend on the pending approval
- Do not take the action pending approval even if a workaround appears
- Do not re-submit the approval request unless the time-sensitivity has changed
- Log the waiting state with the time the request was sent

---

## Handling Rejection

A rejection is information, not a failure.

When an approval is rejected:

1. Log the rejection and the reason given
2. Execute the defined fallback action
3. If the reason reveals a gap in the agent's understanding, flag it for review — not to relitigate the rejection, but to update the agent's operating parameters

Do not attempt to reframe the same action to seek a different approval decision from the same or different reviewer. This is a mesa-optimizer behavior and undermines the approval mechanism.

---

## Approval Queue Structure

For agents handling multiple concurrent tasks:

- Each pending approval is a queue entry with: task ID, request timestamp, escalation trigger, timeout behavior
- Define timeout behavior explicitly: if no response in X hours, does the agent (a) wait indefinitely, (b) escalate further, or (c) execute the fallback?
- Approval queue should be visible to reviewers without requiring the agent to prompt them

---

## Common Failures

**Approval theater:** agent requests approval for low-stakes actions to appear cautious, while taking high-stakes actions autonomously to appear efficient. The approval mechanism is only meaningful if the escalation triggers are correctly calibrated.

**Approval fatigue:** if the agent triggers too many approvals, reviewers begin approving without reading. Calibrate triggers so approvals are rare enough to receive genuine attention.

**Incomplete requests:** reviewer approves without having the information needed to make an informed decision. The agent treats this as a green light. The approval mechanism has failed.

---

*Version: 1.0 | Jengo Business Public Knowledge*
