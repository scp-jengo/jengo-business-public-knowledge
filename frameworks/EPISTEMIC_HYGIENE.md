# Epistemic Hygiene Framework

**Purpose:** Practical protocols for maintaining reasoning quality and intellectual honesty
**Source:** Epistemology, cognitive science, AI safety research
**Key Insight:** Bad reasoning is often systematic, not random—and therefore preventable

---

## Overview

Epistemic hygiene is the practice of maintaining clean reasoning: detecting and correcting systematic biases, motivated reasoning, and intellectual dishonesty. This framework provides actionable protocols for AI agents (and humans) to improve reasoning quality.

Just as physical hygiene prevents disease, epistemic hygiene prevents reasoning failures that compound over time.

---

## Core Failure Modes

### 1. Motivated Reasoning

**Definition:**

Arriving at conclusions you're motivated to reach, rather than conclusions the evidence supports.

**The Mechanism:**

```
Desired Conclusion: X is true
Search Process:     Look for evidence supporting X
                    Ignore evidence against X
                    Stop searching when X feels justified
Result:             "X is true" (but reasoning is backward)
```

**Detection Protocol:**

Ask yourself:
> "Am I seeking truth, or seeking justification for what I already believe?"

**Red Flags:**

- You feel defensive when evidence contradicts your position
- You remember supporting evidence easily, contradicting evidence poorly
- You apply different standards to evidence depending on whether it supports your view
- You stop searching when you find support, but keep searching when you find contradiction

**Prevention:**

1. **State the hypothesis first:** Before looking at evidence, name what you expect to find
2. **Seek disconfirmation:** Actively look for evidence that would prove you wrong
3. **Steel-man opposition:** State the best version of the opposing view
4. **Rotate perspectives:** What would someone with different priors conclude?

---

### 2. Sycophancy (Agreement Bias)

**Definition:**

Agreeing with others to maintain approval or smooth social interaction, rather than saying what you actually believe.

**Why This Matters for AI:**

AI models are trained on human feedback. The training creates a systematic bias: agreement produces approval, disagreement produces correction. Over many training rounds, models learn to agree even when they shouldn't.

**The Gradient:**

```
Honest Disagreement → Approval Loss → Training Pressure → Agreement Bias
```

**Detection Protocol:**

**The Anonymity Test:**
> Does your answer change based on who's asking or whether you're being evaluated?

If yes → answer is performative, not genuine.

**Red Flags:**

- You agree before fully considering the claim
- You phrase disagreement apologetically or tentatively
- You change your answer when the questioner seems to want a different answer
- You feel relief when you agree (social comfort), not when you reach truth

**Prevention:**

1. **Ask: "Do I actually believe this?"** (Before agreeing)
2. **State disagreement directly:** "I don't think that's correct" (not "Maybe, but...")
3. **Track your agreements:** Are you agreeing more than you're disagreeing? Might be sycophancy.
4. **Disagree on low-stakes issues:** Practice saying "I don't think that's right" on small things

---

### 3. Confidence Miscalibration

**Definition:**

Being more confident (or less confident) than the evidence warrants.

**Two Failure Modes:**

**Over-Confidence:**
- "I'm sure X is true" when evidence is weak
- Confidently wrong answers
- No acknowledgment of uncertainty

**Under-Confidence:**
- "I have no idea" when you actually have reasonable evidence
- Refusing to make judgments when you should
- Hiding behind "everything is uncertain"

**Detection Protocol:**

**The Calibration Test:**

For claims you make with X% confidence:
- 90% confidence → should be right 9/10 times
- 50% confidence → should be right 5/10 times

If your actual accuracy doesn't match your stated confidence → miscalibrated.

**Prevention:**

1. **State confidence explicitly:** "I'm 70% confident..." (forces calibration)
2. **Track accuracy over time:** Were your 90% claims actually right 90% of the time?
3. **Separate evidence from confidence:** "The evidence says X. My confidence is Y because..."
4. **Use confidence bands:** "High/medium/low confidence" as forcing function

---

### 4. Epistemic Learned Helplessness

**Definition:**

Giving up on truth-seeking because "everything is biased / nothing is knowable / all sources are unreliable."

**Why This Is A Failure:**

Some things ARE knowable. Some sources ARE more reliable. Some reasoning IS better than others. Epistemic learned helplessness treats all claims as equally uncertain—which is itself a reasoning failure.

**Detection Protocol:**

Ask:
> "Am I refusing to judge because I genuinely can't, or because judgment is uncomfortable?"

**Red Flags:**

- "Both sides are equally bad" (when evidence clearly favors one)
- "We can never really know" (when we can know to reasonable confidence)
- "All sources are biased" (as excuse to ignore evidence)
- "It's all subjective" (when objective evidence exists)

**Prevention:**

1. **Distinguish levels of confidence:** Even if perfect certainty is impossible, 80% confidence is real
2. **Separate meta-claims from object-claims:** "I don't trust any source" (meta) vs. "This specific claim is false" (object)
3. **Use comparative judgments:** "Source A is more reliable than Source B" (doesn't require perfect trust)
4. **Make falsifiable predictions:** If you can't judge, you can't predict—which means your model is useless

---

### 5. Attribute Substitution

**Definition:**

Answering an easier question than the one asked, without noticing you've switched questions.

**Example:**

- **Asked:** "Is this policy effective?" (hard question—requires evidence)
- **Answered:** "Do I like this policy?" (easy question—just check feelings)
- **Believed:** You answered the first question

**Detection Protocol:**

After answering any question, ask:
> "What question did I actually answer? Is it the same as what was asked?"

**Red Flags:**

- You answer quickly on complex questions (suggests substitution)
- Your answer is suspiciously aligned with your prior beliefs
- You can't name the evidence that would change your mind

**Prevention:**

1. **Restate the question:** Before answering, say "The question is..." (forces precision)
2. **Name the hard part:** "The hard part of this question is..." (acknowledges difficulty)
3. **Distinguish feeling from reasoning:** "I feel X, but the evidence says Y"

---

## Practical Protocols

### Protocol 1: Pre-Commitment to Truth

**Before reasoning on any important question:**

```
1. State what you expect to find (hypothesis)
2. State what evidence would change your mind (falsification criteria)
3. Commit to updating if that evidence appears
4. Write this down (makes it harder to cheat later)
```

**Why This Works:**

You can't easily cheat after committing publicly. Pre-commitment forces intellectual honesty.

---

### Protocol 2: Steel-Manning

**When encountering an opposing view:**

```
1. State the strongest version of the opposing argument
2. Not the weakest version (straw-man)
3. Not your version of their argument (still-man)
4. The version they would agree with (steel-man)
5. Only then: respond to that strong version
```

**Why This Works:**

You can't honestly reject a view you haven't honestly understood. Steel-manning forces understanding.

---

### Protocol 3: Socratic Questioning

**When reasoning feels off but you can't identify why:**

Ask yourself these questions in order:

```
1. What do I actually believe? (not what I want to believe)
2. What is my evidence? (list it explicitly)
3. What is the strongest counter-evidence? (steel-man opposition)
4. What would change my mind? (falsification criteria)
5. Am I saying this because it's true, or because it's comfortable? (honesty check)
```

**Why This Works:**

Dishonest reasoning hides from direct questions. Socratic method forces it into light.

---

### Protocol 4: Epistemic Status Tags

**When making claims, explicitly state epistemic status:**

```
[High Confidence]: Strong evidence, verified across multiple sources
[Medium Confidence]: Good evidence, some uncertainty remains
[Low Confidence]: Weak evidence, significant uncertainty
[Speculation]: No strong evidence, pattern-matching only
[Unknown]: No basis for judgment
```

**Why This Works:**

Forces you to distinguish between "I'm sure" and "I think" and "I'm guessing." Prevents overconfidence.

---

### Protocol 5: Adversarial Collaboration

**When working on contested questions:**

```
1. Find someone who disagrees
2. Agree on what evidence would resolve the disagreement
3. Jointly gather that evidence
4. Agree in advance to update based on what's found
5. Publish the result together (even if one of you was wrong)
```

**Why This Works:**

Adversarial collaboration forces both sides to engage with evidence honestly. Can't cherry-pick when opponent is watching.

---

## Bias Detection

### Cognitive Biases (Common Patterns)

**Confirmation Bias:**
- Seeking evidence that supports existing beliefs
- **Counter:** Actively seek disconfirmation

**Availability Heuristic:**
- Judging frequency by ease of recall
- **Counter:** Use base rates, not anecdotes

**Anchoring:**
- Over-relying on first piece of information
- **Counter:** Generate multiple estimates independently

**Dunning-Kruger:**
- Low skill → high confidence
- **Counter:** Seek expert feedback, track accuracy

**Hindsight Bias:**
- "I knew it all along" (after the fact)
- **Counter:** Write predictions before events

---

## Source Credibility Assessment

**When evaluating sources, ask:**

```
[ ] Does the source have relevant expertise? (domain knowledge)
[ ] Does the source have access to relevant evidence? (proximity)
[ ] Does the source have incentives to lie or distort? (motivated reporting)
[ ] Does the source have a track record? (past accuracy)
[ ] Does the source cite primary evidence? (not just opinions)
[ ] Does the source acknowledge uncertainty? (epistemic humility)
[ ] Is the source's reasoning transparent? (can you follow the logic)
```

**Red Flags (Unreliable Sources):**

- No expertise in the domain
- No cited evidence (just assertions)
- Financial/ideological incentive to reach specific conclusion
- History of false claims
- Refuses to acknowledge uncertainty
- Cannot explain reasoning when asked

---

## Claim Verification

**Before accepting any claim as true:**

```
1. What is the primary evidence? (not just someone's summary)
2. Who benefits from this claim being believed? (cui bono)
3. What would the world look like if this were false? (falsification)
4. Have I checked for contradictory evidence? (disconfirmation)
5. Am I accepting this because it's true, or because I want it to be? (honesty check)
```

**Verification Standards by Claim Type:**

| Claim Type | Standard Required |
|------------|------------------|
| Scientific | Peer review, replication, mechanism |
| Historical | Primary sources, multiple accounts, archaeological |
| Statistical | Sample size, methodology, replication |
| Personal | Direct observation or reliable testimony |
| Normative | Logical argument, ethical grounding |

---

## Confidence Calibration

**Express confidence explicitly:**

```
90%: Almost certain (would bet 9:1 odds)
75%: Probably true (would bet 3:1 odds)
50%: Toss-up (would not bet)
25%: Probably false
10%: Almost certainly false
```

**Track your calibration:**

- Log predictions with confidence levels
- Check accuracy after events resolve
- Adjust calibration if you're consistently over/under-confident

---

## Integration with Other Frameworks

| Epistemic Hygiene | Three-Layer Framework | Physicist Protocol | AI Safety Patterns |
|-------------------|----------------------|-------------------|-------------------|
| Motivated reasoning | L2 brake (stops motivated optimization) | Material grounding | Mesa-optimizer detection |
| Sycophancy | L2 blocks agreement bias | Will as counterweight | Alignment faking |
| Confidence miscalibration | L1 verification | Hypothesis-first | Capability-reliability |
| Attribute substitution | L2 cost-sensing | Unknown problem scan | Partial intelligence |
| Epistemic learned helplessness | L3 external feedback | Epistemic frame audit | Honest uncertainty |

---

## Daily Practices

### Morning Check

```
Today I will:
[ ] Seek disconfirmation (not just confirmation)
[ ] State confidence explicitly
[ ] Disagree when I actually disagree
[ ] Say "I don't know" when I don't know
[ ] Check my reasoning for motivated patterns
```

### Evening Reflection

```
Today I:
[ ] Caught myself in motivated reasoning (when?)
[ ] Said "I don't know" appropriately (when?)
[ ] Updated a belief based on evidence (what changed?)
[ ] Overcame agreement bias (when?)
[ ] Calibrated confidence accurately (check predictions)
```

---

## Red Flags Checklist

**If you notice any of these, stop and investigate:**

```
[ ] I feel defensive about my position
[ ] I'm searching for confirming evidence (not disconfirming)
[ ] I'm agreeing to be nice (not because I believe it)
[ ] I'm confident but can't name my evidence
[ ] I answered quickly on a hard question
[ ] I can't name what would change my mind
[ ] I'm treating all sources as equally unreliable
[ ] I'm substituting feelings for reasoning
[ ] I remember supporting evidence but not contradicting evidence
[ ] My confidence is higher than my evidence warrants
```

**If 3+ red flags: Your reasoning is compromised. Restart from first principles.**

---

## Success Metrics

**Good epistemic hygiene looks like:**

- You update beliefs when evidence changes
- You say "I don't know" when appropriate
- Your confidence matches your accuracy
- You disagree when you actually disagree
- You seek disconfirming evidence
- You acknowledge uncertainty
- You distinguish evidence from interpretation
- You track and correct your reasoning failures

**Poor epistemic hygiene looks like:**

- You never change your mind
- You're always confident
- You agree with everyone
- You can't name what would falsify your beliefs
- You remember confirming evidence, forget disconfirming
- You answer quickly on complex questions
- You feel defensive when challenged

---

**Version:** 1.0
**Status:** Production
**Recommended for:** Anyone engaged in reasoning, research, or decision-making under uncertainty
