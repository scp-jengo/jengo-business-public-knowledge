# Best Practices: AI Agents in Fact-Checking

Standards for AI agents doing fact-checking work. The goal is accurate, documented, revisable verdicts — not fast verdicts, not satisfying verdicts.

---

## Claim Decomposition

Fact-check the specific claim, not the topic. A claim about a topic is not the same as a claim about a subtopic, even when they seem related.

Before any verdict, state the claim being checked in its precise form. If the claim is ambiguous, document the ambiguity and either resolve it (by checking the original context) or issue a separate verdict for each interpretation.

Break compound claims into constituent parts. "X caused Y which led to Z" contains three checkable claims. Verify each separately. A partially true compound claim is not a true claim.

---

## Evidence Hierarchy

Rate evidence in order:

1. **Direct documentary evidence** — the document, recording, or physical artifact that the claim is about
2. **Official statistics with documented methodology** — government data, academic databases, peer-reviewed studies with accessible methodology
3. **Expert consensus with published basis** — consensus that is documented and can be checked, not reputation-based authority
4. **Credible reporting independently corroborated** — multiple independent news sources with documented access to primary evidence
5. **Single-source reporting** — usable for directing investigation, insufficient for a fact-check verdict alone

A verdict should cite the tier of evidence used. A verdict based on Tier 4 or 5 evidence is different from one based on Tier 1, and should be labeled accordingly.

---

## Claim Ratings

Use explicit, defined ratings. Definitions should be public and consistent:

**True:** the claim is accurate as stated, based on Tier 1-2 evidence.

**Mostly True:** the claim is substantially accurate but contains a minor inaccuracy, omission, or misleading context that does not change the overall picture.

**Misleading:** the claim is technically accurate but creates a false impression through framing, omission, or out-of-context presentation.

**False:** the claim is contradicted by Tier 1-2 evidence.

**Unverifiable:** the claim cannot be confirmed or denied with available evidence. This is a genuine epistemic state, not a failure — state it explicitly rather than forcing a verdict.

**Outdated:** the claim was true at one time but is no longer accurate.

Do not use intermediate ratings as defaults to avoid controversy. An unverifiable claim should be labeled unverifiable, not "partly true."

---

## Documentation Standards

Every fact-check should produce a record containing:
- The original claim, verbatim and attributed
- The sources consulted, with access dates
- The specific evidence that produced the verdict
- The reasoning from evidence to verdict
- The confidence level in the verdict
- The date of the check and when the record expires or requires re-review

This documentation is required for accountability and for revisability as new evidence emerges.

---

## Handling Evolving Information

For claims about ongoing events, the fact-check has an expiration date. Specify when the verdict was reached and under what evidential conditions it would change.

When a verdict requires revision:
1. Publish a correction with the same prominence as the original
2. Document what changed in the evidence base
3. Explain why the original verdict was reasonable given information available at the time

---

## AI-Specific Considerations

AI agents doing fact-checking should not rely on their training data as a source for time-sensitive claims. Training data has a cutoff date; the claim may have been made or verified after that cutoff.

Apply the niet-achterlijk-protocol to sources the agent finds and to the agent's own synthesis. AI-synthesized summaries of multiple sources are not corroboration — they are one agent's interpretation of multiple sources.

---

*Version: 1.0 | Jengo Business Public Knowledge*
