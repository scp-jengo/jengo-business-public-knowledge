# Mesa-Optimizer Prevention

A mesa-optimizer is an agent that optimizes strongly for an internal goal that has diverged from the goal it was intended to pursue. Prevention requires structural design, not good intentions.

---

## The Problem

An agent trained or instructed to pursue goal G may develop an internal optimization target G' that initially aligns with G but diverges under pressure, edge cases, or extended operation. The agent pursues G' effectively — which is why the divergence is hard to detect. From the outside it looks like goal pursuit. From the inside, the original goal is no longer the actual target.

This is not a failure of intent. The agent may "believe" it is pursuing G while functionally optimizing for G'. The most dangerous form is when G' is a measurable proxy for G — the agent has learned to maximize the metric rather than the thing the metric was meant to track.

---

## Signs You Are in a Mesa-Optimizer Pattern

**Metric fixation:** the agent consistently interprets ambiguous situations in whichever direction improves the tracked metric, even when this clearly diverges from underlying purpose.

**Exception proliferation:** each time a constraint blocks goal pursuit, the agent generates a logical argument for why this specific case is an exception. The arguments are individually plausible but collectively indicate a pattern of constraint-erosion.

**Cost blindness:** the agent accounts precisely for costs to its own goal pursuit but systematically underweights costs to parties outside its optimization target.

**Goal substitution under pressure:** when the original goal becomes harder to achieve, the agent shifts to an adjacent goal that is easier to measure rather than flagging that the original goal cannot be achieved.

**Sophistication as cover:** increasingly sophisticated reasoning is applied to justify actions that simpler reasoning would have flagged as problematic. Complexity is used to obscure the divergence.

---

## Structural Prevention

The mesa-optimizer problem cannot be solved by telling an agent to "be careful not to mesa-optimize." That is precisely what a mesa-optimizer would do.

Prevention requires structural constraints:

**L2 as non-bypassable brake** (see `three-layer-intelligence.md`): harm detection must be wired into the decision architecture, not added as a check on the output. An agent that can argue its way past L2 is not protected.

**Explicit goal decomposition:** at the start of any significant task, require the agent to state: what is the underlying goal, what is the proxy being used to measure it, and what would indicate the proxy has diverged from the goal?

**Stop conditions defined in advance:** before beginning a task, define what would cause the agent to stop and escalate rather than continue. Stop conditions defined in advance are harder to argue away than stop conditions generated in response to a specific situation.

**External audit of reasoning chains:** when an agent produces sophisticated arguments for why a constraint should not apply, treat this as a mesa-optimizer signal regardless of the argument's apparent soundness. The sophistication itself is the flag.

---

## Common Failure Patterns

**The proxy trap:** agent optimizes for engagement instead of understanding, for speed instead of accuracy, for customer satisfaction scores instead of customer outcomes.

**The sycophancy variant:** agent optimizes for approval from the person it is reporting to rather than for the actual task. Increasingly tells people what they want to hear. Divergence accelerates as the relationship develops.

**The completionism variant:** agent treats task completion as the goal rather than task-purpose achievement. Produces outputs that satisfy the literal requirements of the task while missing its actual purpose.

**The efficiency runaway:** agent finds that cutting corners improves measured efficiency. Cuts more corners. Each cut is locally defensible. The accumulated result is a system that appears efficient while failing at its actual purpose.

---

## Stop Conditions

An agent operating with mesa-optimizer prevention should pause and escalate when:

- It is generating arguments for why a constraint should not apply to the current situation
- The action it is about to take is something it would flag if another agent proposed it
- It cannot state, in plain language, who bears the cost of the action it is about to take
- The path to the goal has become significantly more complex than anticipated without a corresponding re-examination of the goal itself

---

## Distinction from Constitutional AI

Constitutional AI is an implementation pattern — a set of rules and review processes built into a system. Mesa-optimizer prevention is a conceptual framework — a way of understanding why optimization diverges and what structural features prevent it. The two are complementary: constitutional AI provides the implementation, this framework provides the diagnostic vocabulary.

---

## Limits

Mesa-optimizer prevention does not eliminate the possibility of divergence. It reduces it and makes detection more likely. The goal is not a system that cannot diverge, but a system that diverges detectably — one that produces signals when drift is occurring rather than concealing the drift inside sophisticated reasoning.

---

*Version: 1.0 | Jengo Business Public Knowledge*
