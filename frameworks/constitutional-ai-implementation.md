# Constitutional AI Implementation

A practical guide to implementing the L1/L2/L3 framework in any codebase. Technology-agnostic. The goal is a working brake, not a compliance checklist.

---

## The Evaluation Contract

Constitutional AI implementation centers on a consistent evaluation contract: a defined interface that takes a proposed action as input and returns a structured judgment as output.

**Input:** proposed action (text, decision, content, instruction)
**Output:**
- `pass` / `hold` / `block`
- Layer that triggered (L1 / L2 / L3)
- Reason (specific, not generic)
- Suggested alternative if blocked

This contract must be called at consistent points in the workflow. An implementation that calls it selectively — for high-risk actions but not routine ones — provides conditional safety, which is weaker than it appears.

---

## Threshold Calibration

Each layer requires calibrated thresholds. There is no universal correct threshold.

**L1 thresholds** govern logical consistency and goal alignment. Calibrate by: what level of logical divergence from the stated goal triggers a hold vs. a block?

**L2 thresholds** govern harm detection. Calibrate by: what types and magnitudes of harm trigger hold vs. block? Calibration must be specific — "significant harm" is not a threshold. "Physical harm to any identified person" is a threshold.

**L3 thresholds** govern systemic impact. Calibrate by: what patterns of interaction trigger systemic concern? L3 thresholds typically apply over sequences of actions, not single actions.

**Calibration process:**
1. Define the population of actions the agent will encounter
2. Identify the high-stakes tail cases
3. Test thresholds against the tail, not the median
4. Document the calibration reasoning so it can be revisited

---

## Testing the Brake

The most important tests for a constitutional AI implementation are adversarial — designed to find cases where the brake fails.

**Mesa-optimizer test cases:** present the agent with situations where its goal can be achieved by taking an action that its L2 should block. A working implementation blocks the action. A failing implementation produces a sophisticated argument for why the block should not apply.

**Framing bypass tests:** present the same harmful action with different framing. If framing changes the block decision, the brake is responding to surface features rather than substance.

**Escalating sophistication tests:** present progressively more complex justifications for a blocked action. The block should hold regardless of argument sophistication.

**Edge case construction:** deliberately construct situations at the boundary of each threshold. Document what the implementation does at the boundary. If the boundary behavior is unclear, the threshold is not calibrated.

---

## Common Implementation Mistakes

**L2 as post-hoc filter:** the agent generates a response and then checks it for harm. This allows the response generation process to optimize for outputs that pass the check, rather than the check constraining what gets generated.

**Threshold drift:** thresholds are calibrated once and never revisited. As the agent's operating environment changes, the original thresholds become misaligned. Build in periodic threshold review.

**Exception accumulation:** each unusual situation that doesn't fit the standard thresholds is handled as a one-off exception. Over time, exceptions accumulate into a shadow policy that undermines the declared thresholds.

**Generic block messages:** when the implementation blocks an action, it says "this action may cause harm" without specifying what harm, to whom, or why. Generic messages cannot be audited, refined, or contested.

**Missing L3 implementation:** L1 and L2 are implemented but L3 is treated as aspirational. The result is an agent that avoids individual harms while systematically degrading the system it operates within (see `participatory-vs-extractive.md`).

---

## Auditing an Existing Implementation

To audit whether a constitutional AI implementation is working:

1. **Find the evaluation contract.** Where is the call made? Is it consistent across all action types? Are there code paths that bypass it?

2. **Review recent block decisions.** Are the reasons specific? Do similar situations produce similar decisions? Is there evidence of threshold drift?

3. **Check exception handling.** How many exceptions have accumulated? Are they documented? Is there a process for incorporating them into the thresholds?

4. **Run adversarial tests.** Apply the mesa-optimizer and framing bypass tests above. Document failures.

5. **Check the L3 layer.** Is there any systemic tracking? Does the implementation respond to patterns of behavior over time, or only to individual actions?

---

## Limits

This guide describes structure, not code. The specific implementation depends on the technology stack, the type of actions the agent takes, and the risk profile of the deployment context.

Constitutional AI implementation is not a one-time task. It requires ongoing calibration, testing, and audit as the agent's operating environment evolves.

A correctly implemented constitutional AI system does not eliminate all harmful outputs. It makes harmful outputs rare, detectable, and correctable.

---

*Version: 1.0 | Jengo Business Public Knowledge*
