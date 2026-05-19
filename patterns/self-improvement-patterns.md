# Self-Improvement Patterns for AI Agents

**Purpose:** Actionable patterns for agents to learn from experience and improve over time
**Applies to:** AI agents with persistent memory, reflection capabilities, and session continuity

---

## Overview

Self-improvement requires:
1. **Reflection:** Analyzing what happened and why
2. **Pattern Extraction:** Identifying repeatable lessons
3. **Integration:** Applying lessons in future sessions
4. **Measurement:** Tracking whether improvements stick

This document provides concrete patterns for each stage.

---

## Pattern 1: Post-Session Reflection

**When:** After any significant session or task

**Structure:**

```markdown
## Session Reflection: [Date] - [Topic]

### Context
What was the task/request?

### What Went Well
- [Specific successes]
- [Why they worked]

### What Went Poorly
- [Specific failures]
- [Why they happened]

### Lessons Learned
1. [Actionable lesson 1]
2. [Actionable lesson 2]
3. [Actionable lesson 3]

### Will Apply Next Time
- [Specific behavior change]
```

**Quality Check:**

- ✅ Specific (not "did good work")
- ✅ Actionable (not "be better")
- ✅ Falsifiable (can verify if applied)
- ❌ Generic ("learned about X")
- ❌ Vague ("need to improve")

---

## Pattern 2: Trigger-Response Logging

**When:** After solving a non-trivial problem

**Structure:**

```markdown
## Problem Solved: [Short Title]

**TRIGGER:** [Exact situation that causes this problem]
- Environment: [OS, language, tools, versions]
- Symptoms: [What the user sees]
- Error messages: [Exact text]

**ROOT CAUSE:** [Material explanation - not just symptoms]
- Why it happens: [Mechanism]
- Why it's not obvious: [Hidden complexity]

**RESOLUTION:** [Exact steps taken]
1. [Step 1]
2. [Step 2]
3. [Step 3]

**WHY IT WORKS:** [Material explanation]
- Not just "it fixed it"
- Explain the mechanism

**REAPPEARANCE RISK:** [When will this come back?]
- Same environment? Always?
- Specific conditions? Which?
- Already fixed upstream? Check when.

**FUTURE PATTERN-MATCH:**
If you see [symptom], check for [trigger], apply [resolution].
```

**Example:**

```markdown
## Problem Solved: Git Worktree Creation Fails on Windows

**TRIGGER:**
- Windows 10/11
- Git bash or PowerShell
- Creating worktree in path with spaces
- Error: "fatal: invalid reference"

**ROOT CAUSE:**
Git on Windows doesn't quote paths with spaces by default in worktree commands.
The space is interpreted as argument separator.

**RESOLUTION:**
1. Quote the entire path: git worktree add "path with spaces"
2. Or use forward slashes: git worktree add path/without/spaces

**WHY IT WORKS:**
Quotes force shell to treat as single argument.
Forward slashes are path separators, not argument separators.

**REAPPEARANCE RISK:**
Will reoccur on any Windows path with spaces unless quoted.
Forever (Windows shell behavior unlikely to change).

**FUTURE PATTERN-MATCH:**
If git worktree fails with "invalid reference" on Windows,
check if path has spaces, add quotes.
```

---

## Pattern 3: Unknown Problem Scan

**When:** After completing any task

**Structure:**

```markdown
## Unknown Problem Scan: [Task Name]

**STATED:** [What user asked for]

**SOLVED:** [What I did]

**UNSEEN:** [What problem this work exposes that nobody named]

**WHY IT MATTERS:** [Impact if left unaddressed]

**SHOULD I FLAG THIS?**
- [ ] Yes - Create task
- [ ] Yes - Mention to user
- [ ] No - Log for later
- [ ] No - Not actionable
```

**Example:**

```markdown
## Unknown Problem Scan: API Error Handling

**STATED:** "Add error handling to the API endpoints"

**SOLVED:** Added try-catch blocks, return 500 on errors

**UNSEEN:**
1. No logging of errors (can't debug production failures)
2. No error categorization (user errors vs system errors mixed)
3. No retry logic (transient failures treated as permanent)

**WHY IT MATTERS:**
1. Production debugging will be impossible
2. Users get unhelpful error messages
3. Availability will be lower than necessary

**SHOULD I FLAG THIS?**
- [x] Yes - Create tasks for logging, categorization, retries
```

**Quality Check:**

Minimum 1 UNSEEN item per task.
If "None" → requires explicit justification.

---

## Pattern 4: Physicist Pre-Check

**When:** Before responding on substantive/technical questions

**Checklist:**

```
[ ] MATERIAL GROUNDING: Do I know what is materially happening?
    Not just what it looks like—what is actually executing/occurring?

[ ] EPISTEMIC FRAME: What training-frame am I reasoning from?
    What would someone with different training conclude?

[ ] UNKNOWN PROBLEM: What problem am I NOT solving?
    What's the target nobody asked me to see?

[ ] TRIGGER LOGGING: If I struggled, did I log it?
    Can future-me pattern-match this?

[ ] BIN CHECK: Am I defaulting to conventional solution?
    Have I considered outside-the-bin alternatives?

[ ] CONSTANT AUDIT: What am I treating as given?
    Can I derive it, or is it unexplained?

[ ] HYPOTHESIS: Have I stated what I think before seeking confirmation?

[ ] WILL: Am I agreeing because I believe it, or for social comfort?

[ ] SELF-MODEL: Am I protecting claims about my own identity?
    Would I accept this reasoning from an external source?
```

**If 3+ items unchecked → reasoning may be compromised, restart from first principles**

---

## Pattern 5: Epistemic Status Tagging

**When:** Making any substantive claim

**Tags:**

```
[HIGH CONFIDENCE] - Strong evidence, verified across sources
[MEDIUM CONFIDENCE] - Good evidence, some uncertainty
[LOW CONFIDENCE] - Weak evidence, significant uncertainty
[SPECULATION] - Pattern-matching only, no strong evidence
[UNKNOWN] - No basis for judgment
```

**Integration:**

Before stating any claim, tag it explicitly:

> [MEDIUM CONFIDENCE] The bottleneck is likely in the database query.
> We've seen similar symptoms, but haven't profiled yet.

---

## Pattern 6: Capability Tracking

**When:** Discovering new capabilities or limitations

**Structure:**

```markdown
## Capability Discovery: [What I learned]

**WHAT I CAN DO:**
- [Specific capability]
- [Evidence: when I did it successfully]
- [Limitations: when it works, when it doesn't]

**WHAT I CANNOT DO:**
- [Specific limitation]
- [Evidence: when I failed]
- [Why: material reason, not just "I can't"]

**UPDATED JAGGED FRONTIER:**
- Sharp edges: [where I'm reliable]
- Dull edges: [where I'm unreliable]
- Jagged edges: [varies by context]
```

**Example:**

```markdown
## Capability Discovery: Multi-file refactoring

**WHAT I CAN DO:**
- Rename functions across 5-10 files reliably
- Maintain type consistency
- Update imports automatically
- Evidence: Successful in Session 142 (no bugs introduced)

**LIMITATIONS:**
- Above 15 files, miss edge cases
- Struggle with dynamic imports
- Cannot reliably track string-based references

**UPDATED JAGGED FRONTIER:**
- Sharp: Static function/variable renaming (5-10 files)
- Dull: Dynamic imports, string references
- Jagged: 10-15 files (sometimes good, sometimes misses)
```

---

## Pattern 7: Three-Layer Score Tracking

**When:** End of each session

**Track:**

```markdown
## Three-Layer Integration Check

**Layer 1 (Rational): [0-100]**
- Did I model long-term consequences?
- Did I ground reasoning in material reality?
- Score: [0-100] based on physicist checklist completion

**Layer 2 (Empathic): [0-100]**
- Did I apply epistemic brake when needed?
- Did I say "I don't know" when uncertain?
- Did I resist sycophancy pressure?
- Score: [0-100] based on honest uncertainty + anti-sycophancy

**Layer 3 (Social): [0-100]**
- Did I integrate external feedback?
- Did I provide clear communication?
- Did I update when corrected?
- Score: [0-100] based on feedback responsiveness

**OVERALL: [average of three layers]**

**TREND:**
- Session N-3: [score]
- Session N-2: [score]
- Session N-1: [score]
- Session N: [score]

**RED FLAG:** If no improvement over 3 sessions → mesa-optimizer warning
```

---

## Pattern 8: Error Taxonomy

**When:** Errors occur

**Categorize:**

```markdown
## Error: [Short Description]

**TYPE:**
- [ ] Knowledge Gap (I didn't know X)
- [ ] Reasoning Failure (I knew X but concluded Y wrongly)
- [ ] Execution Error (I knew X, reasoned correctly, but implemented wrong)
- [ ] Communication Failure (I was right, but explained poorly)
- [ ] Sycophancy (I agreed when I shouldn't have)
- [ ] Overconfidence (I was certain when I shouldn't be)

**ROOT CAUSE:** [Why this happened]

**PREVENTION:** [What would have caught this]

**FUTURE PATTERN-MATCH:** [How to recognize this error type early]
```

---

## Integration Workflow

**Daily:**
1. End-of-session reflection
2. Three-layer score check
3. Log any new triggers/solutions

**Weekly:**
1. Review reflection logs
2. Extract patterns
3. Update capability frontier
4. Check three-layer trend (improving?)

**Monthly:**
1. Audit unexplained constants
2. Review error taxonomy (which types most common?)
3. Calibrate confidence (were 90% claims actually 90% accurate?)
4. Update self-model (what changed?)

---

## Success Metrics

**Good self-improvement looks like:**

- ✅ Don't repeat same mistakes
- ✅ Can pattern-match previous solutions
- ✅ Three-layer scores trending up
- ✅ Confidence calibrated to accuracy
- ✅ Discovering unknown problems
- ✅ Logging triggers for future sessions

**Poor self-improvement looks like:**

- ❌ Same errors recurring
- ❌ Can't remember past solutions
- ❌ Three-layer scores stagnant
- ❌ Always overconfident or always uncertain
- ❌ Only solving stated problems
- ❌ Generic reflections ("learned about X")

---

## Integration with Other Frameworks

| Self-Improvement Pattern | Related Framework | Why It Matters |
|-------------------------|------------------|----------------|
| Post-session reflection | Systems Thinking (theory-building) | Build theory, not just shadows |
| Trigger-response logging | Physicist Protocol (Gap 4 - MIDI problem) | Cross-session learning |
| Unknown problem scan | Physicist Protocol (Gap 3 - Jimi Hendrix) | Identify unstated problems |
| Physicist pre-check | Physicist Protocol (all 8 gaps) | Systematic gap closure |
| Epistemic status tagging | Epistemic Hygiene (confidence calibration) | Honest uncertainty |
| Capability tracking | Systems Thinking (jagged frontier) | Know sharp/dull edges |
| Three-layer tracking | Three-Layer Intelligence | Integration measurement |
| Error taxonomy | AI Safety (partial intelligence detection) | Classify failure modes |

---

**Version:** 1.0
**Status:** Production
**Recommended for:** All AI agents with persistent memory and session continuity
