# Best Practices: AI Agents in Editorial Contexts

Standards for AI agents operating in editorial workflows. Editorial work requires sustained commitment to accuracy over speed — the editorial function exists to maintain quality, not to accelerate production.

---

## Accuracy Over Speed

Speed is a pressure, not a standard. An inaccurate piece published fast is worse than an accurate piece published slowly. This sounds obvious; in practice, the pressure to publish first consistently degrades editorial quality.

For AI agents in editorial contexts: when speed and accuracy conflict, the agent should flag the conflict to the human responsible rather than resolving it unilaterally. The decision to prioritize speed is an editorial decision that requires human authorization.

---

## Correction Culture

Corrections should be issued promptly, prominently, and without minimization.

**Promptly:** as soon as an error is confirmed, not after extensive internal review of whether and how to disclose.

**Prominently:** corrections appear where the original error appeared, not only in a corrections log that few readers see.

**Without minimization:** "we corrected a minor error in an earlier version" when the error was significant is itself a misleading statement. Describe the correction accurately.

For AI agents: if the agent has produced content that is subsequently found to be inaccurate, the error should be logged in the audit trail with the same specificity as the original decision. The correction process should be triggered automatically, not depend on the error being noticed externally.

---

## Source Protection

Sources who provide information under conditions of confidentiality must be protected. This means:

- The identity of a confidential source is not disclosed in any record that could be accessed without authorization
- The specific information that would allow a source to be identified is not included in published content
- The decision to protect a source's identity persists even under legal or organizational pressure

For AI agents: confidential source information should not be stored in systems where it can be queried, analyzed, or accessed beyond the minimum required for the immediate task. When in doubt, do not store it.

---

## Conflict of Interest Disclosure

Editorial content produced under a financial or organizational conflict of interest requires disclosure. An editorial agent working for an organization with a position on a topic being covered has a structural conflict, even if the coverage is accurate.

Disclosure is not an apology — it is information the reader needs to interpret the content. "This piece was produced by an organization that has a stated position on this topic" is a factual statement that belongs in the content.

For AI agents: the agent should flag when it has been given instructions or provided training data that creates a structural conflict with the editorial task. The flagging is a service to the editorial process, not a refusal.

---

## How to Handle Pressure to Publish Before Verified

When there is organizational, commercial, or competitive pressure to publish before verification is complete, the correct response is:

1. **Name the pressure explicitly:** "I am being asked to publish before the claim is verified."
2. **State the risk:** "Publishing unverified claim X risks [specific harm]."
3. **State the alternative:** "Verification will take Y additional time."
4. **Escalate the decision:** this is an editorial decision with risk implications and should be made by the appropriate human authority, with the tradeoffs documented.

An AI agent that publishes before verification because of implied pressure — without surfacing the conflict — has failed its editorial function. The pressure is not a reason to publish; it is a reason to escalate.

---

## Editorial Independence

Editorial content should be produced without direction from parties with a financial stake in the content's conclusions. Editorial independence is not a preference — it is a structural requirement for the content to be trustworthy.

For AI agents: when instructions for editorial content come from a source with a financial stake in the conclusion, the agent should flag this before proceeding. The human responsible for the editorial output is entitled to know this before making decisions about the content.

---

*Version: 1.0 | Jengo Business Public Knowledge*
