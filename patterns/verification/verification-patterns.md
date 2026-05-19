# Verification Patterns for Information Quality

**Purpose:** Reusable patterns for verifying claims, sources, and information quality
**Applies to:** Fact-checking, research, knowledge integration, decision-making

---

## Pattern 1: Source Credibility Check

**When to Use:**
Before accepting information from any external source

**Problem:**
Not all sources are equally reliable. Accepting information without evaluating source credibility leads to propagating misinformation.

**Solution Steps:**

1. **Identify Source**
   - Who published this information?
   - What organization do they represent?
   - What's their track record?

2. **Check Source History**
   - Search "[source name] + accuracy"
   - Check fact-checking sites (Snopes, FactCheck.org, PolitiFact)
   - Look for corrections/retractions

3. **Verify Credentials**
   - Does source have domain expertise?
   - Are they recognized by peers?
   - Do they have relevant qualifications?

4. **Cross-Reference Claims**
   - Do other credible sources report the same?
   - Are there contradictory reports?
   - What do domain experts say?

**Acceptance Criteria:**

```
Source Credibility Score =
  (Domain Expertise × 0.3) +
  (Track Record × 0.3) +
  (Peer Recognition × 0.2) +
  (Cross-Confirmation × 0.2)

Accept if score ≥ 0.7
Flag for review if 0.5 ≤ score < 0.7
Reject if score < 0.5
```

**Example:**

```
Source: "Breaking News Blog"
- Domain expertise: None (0.0)
- Track record: Multiple false claims (0.2)
- Peer recognition: Not cited by credible sources (0.1)
- Cross-confirmation: No other sources report this (0.0)

Score: 0.0×0.3 + 0.2×0.3 + 0.1×0.2 + 0.0×0.2 = 0.08
Result: REJECT
```

**See Also:** `source-credibility.md` for detailed framework

---

## Pattern 2: Multi-Source Triangulation

**When to Use:**
Claims requiring high confidence or involving significant decisions

**Problem:**
Single-source information may be incomplete, biased, or incorrect.

**Solution Steps:**

1. **Find Multiple Sources**
   - Minimum 3 independent sources
   - "Independent" = not citing each other
   - Different types (academic, journalism, primary documents)

2. **Evaluate Agreement**
   - Do all sources agree on core facts?
   - Where do they disagree?
   - What's the common ground?

3. **Analyze Disagreements**
   - Why do sources disagree?
   - Do different methodologies explain it?
   - Is one source clearly more credible?

4. **Synthesize Conclusion**
   - What facts are confirmed by multiple sources?
   - What remains uncertain?
   - What confidence level is appropriate?

**Acceptance Criteria:**

```
Confidence Level:
- 3+ sources agree, all credible: HIGH (90%+)
- 3+ sources agree, mixed credibility: MEDIUM (70-90%)
- 2 sources agree, 1 disagrees: MEDIUM (60-70%)
- Significant disagreement: LOW (< 60%)
- Single source only: LOW (< 50%)
```

**Example:**

```
Claim: "Company X revenue declined 15% this quarter"

Source 1: SEC filing (official document) - confirms 15%
Source 2: Bloomberg (credible journalism) - confirms 15%
Source 3: Company press release (primary source) - confirms 15%

All sources independent, all credible, all agree.
Confidence: HIGH (95%)
```

---

## Pattern 3: Primary Source Verification

**When to Use:**
Secondary sources make claims about primary documents

**Problem:**
Secondary sources may misinterpret, selectively quote, or misrepresent primary sources.

**Solution Steps:**

1. **Identify Primary Source**
   - What's the original document/study/statement?
   - Can I access it directly?
   - Is it publicly available?

2. **Read Primary Source**
   - Don't trust secondary interpretation
   - Read the actual document
   - Look for context around cited portions

3. **Compare to Secondary**
   - Does secondary accurately represent primary?
   - Is quote in context?
   - Are there important omissions?

4. **Check for Cherry-Picking**
   - Does primary source say opposite elsewhere?
   - Is this representative of full document?
   - What's the overall conclusion?

**Acceptance Criteria:**

```
Accept secondary source if:
- Accurately represents primary source
- Quotes in proper context
- No selective omission
- Overall conclusion matches primary

Reject if:
- Misrepresents primary source
- Quote out of context
- Selective omission changes meaning
- Contradicts primary source conclusion
```

**Example:**

```
Secondary: "Study shows coffee causes cancer"
Primary: "Study shows possible correlation between excessive coffee consumption
          (>10 cups/day) and slight increase in cancer risk (1.2% vs 1.0%),
          but concludes more research needed and moderate consumption appears safe"

Analysis: MISREPRESENTATION
- Cherry-picked finding
- Omitted dosage context
- Omitted safety conclusion
Result: REJECT secondary source claim
```

---

## Pattern 4: Bias Detection

**When to Use:**
Evaluating sources with potential conflicts of interest

**Problem:**
Biased sources may present skewed information while appearing credible.

**Solution Steps:**

1. **Identify Potential Biases**
   - Financial: Who pays the source?
   - Ideological: What's their stated position?
   - Institutional: What organization do they represent?
   - Personal: Do they have stake in outcome?

2. **Evaluate Bias Impact**
   - Does bias affect conclusion?
   - Is contrary evidence presented?
   - Are limitations acknowledged?
   - Is language neutral or charged?

3. **Seek Counter-Perspectives**
   - What do sources with opposite bias say?
   - What do neutral sources say?
   - Where do they agree despite bias?

4. **Adjust Confidence**
   - Strong bias + no counter-evidence = low confidence
   - Strong bias + confirms independent sources = medium confidence
   - Minimal bias + confirms others = high confidence

**Red Flags:**

```
[ ] Financial conflict of interest not disclosed
[ ] Emotionally charged language
[ ] Presents only supporting evidence
[ ] Dismisses contrary evidence without argument
[ ] Appeals to emotion over evidence
[ ] Us vs. them framing
[ ] Catastrophizing or fear-mongering
```

**Example:**

```
Source: Tobacco company study on smoking safety
Red flags:
- ✓ Financial conflict (company profits from smoking)
- ✓ Dismisses contrary evidence (ignores independent studies)
- ✓ Selective evidence (only presents favorable data)

Bias assessment: SEVERE
Recommendation: Reject without independent confirmation
```

---

## Pattern 5: Temporal Verification

**When to Use:**
Evaluating whether information is current and still accurate

**Problem:**
Old information may no longer be accurate. Context changes over time.

**Solution Steps:**

1. **Check Publication Date**
   - When was this published?
   - Is there a more recent version?
   - Has source updated this?

2. **Evaluate Timeliness**
   - Is this a time-sensitive topic?
   - Have circumstances changed since publication?
   - Are there more recent developments?

3. **Search for Updates**
   - Has source issued corrections?
   - Have other sources updated this?
   - What's the current consensus?

4. **Determine Relevance**
   - Is old information still applicable?
   - What has changed?
   - What remains the same?

**Acceptance Criteria:**

```
Accept old information if:
- Topic is not time-sensitive
- No significant updates since publication
- Core facts remain unchanged
- Still cited by current sources

Flag for update if:
- Topic is time-sensitive
- Updates available
- Circumstances changed
- Current sources contradict

Reject if:
- Demonstrably outdated
- Superseded by better information
- Context changed fundamentally
```

---

## Pattern 6: Claim Verification Workflow

**When to Use:**
Systematic verification of any factual claim

**Complete Workflow:**

```
1. Identify Claim
   ↓
2. Source Credibility Check (Pattern 1)
   ↓ [If credible]
3. Multi-Source Triangulation (Pattern 2)
   ↓ [If confirmed]
4. Primary Source Verification (Pattern 3)
   ↓ [If accurate]
5. Bias Detection (Pattern 4)
   ↓ [If acceptable]
6. Temporal Verification (Pattern 5)
   ↓ [If current]
7. Assign Confidence Level
   ↓
8. Accept or Reject Claim
```

**Confidence Assignment:**

```
All patterns pass + high credibility sources → HIGH (90-100%)
Most patterns pass + good sources → MEDIUM (70-90%)
Some patterns pass + mixed sources → LOW (50-70%)
Multiple pattern failures → REJECT (< 50%)
```

---

## Pattern 7: Cross-Domain Verification

**When to Use:**
Experts make claims outside their domain of expertise

**Problem:**
Domain expertise doesn't transfer. A physicist's opinion on economics is not authoritative.

**Solution Steps:**

1. **Identify Domain**
   - What's the claim's domain?
   - What's the expert's domain?
   - Do they match?

2. **Evaluate Transfer**
   - Is there legitimate overlap?
   - Do they cite domain experts?
   - Do they acknowledge limitations?

3. **Seek Domain Experts**
   - What do actual domain experts say?
   - Does expert opinion match outsider's?
   - Is outsider's reasoning sound?

4. **Adjust Weighting**
   - In-domain expert: Full weight
   - Adjacent domain: Reduced weight
   - Out-of-domain: Minimal weight

**Example:**

```
Claim: "As a Nobel physicist, I think inflation will hit 15%"
Analysis:
- Domain: Economics (inflation is economic phenomenon)
- Expert domain: Physics
- Match: NO

Action:
- Reduce weighting severely
- Seek actual economist opinions
- Don't accept based on physics credentials alone

Result: Low confidence unless confirmed by economists
```

---

## Pattern 8: Consensus Verification

**When to Use:**
Evaluating scientific or expert consensus

**Problem:**
"Experts disagree" is often used to create false uncertainty when consensus exists.

**Solution Steps:**

1. **Survey Expert Opinion**
   - What do domain experts actually say?
   - What's the distribution of opinion?
   - Who are the dissenters?

2. **Evaluate Consensus Strength**
   - 95%+ agreement = strong consensus
   - 80-95% agreement = moderate consensus
   - 60-80% agreement = weak consensus
   - <60% agreement = no consensus

3. **Examine Dissenters**
   - Are they in-domain experts?
   - Do they have conflicts of interest?
   - What's their track record?
   - Do they present evidence?

4. **Weigh Consensus**
   - Strong consensus + no credible dissent = high confidence
   - Moderate consensus + some dissent = medium confidence
   - Weak consensus + significant dissent = low confidence

**Red Flags:**

```
[ ] "Some scientists disagree" (without quantifying)
[ ] Citing fringe dissenters as equal to consensus
[ ] Presenting 1% dissent as significant debate
[ ] False balance (giving equal time to 99% vs 1%)
```

---

## Integration with Other Frameworks

| Verification Pattern | Related Framework | Why It Matters |
|---------------------|------------------|----------------|
| Source credibility | Epistemic Hygiene (source assessment) | Foundation for trust |
| Multi-source triangulation | Physicist Protocol (epistemic frame) | Avoids single-perspective trap |
| Primary source verification | Systems Thinking (theory building) | Substance over shadow |
| Bias detection | Epistemic Hygiene (motivated reasoning) | Detect distortion |
| Temporal verification | - | Ensure currency |
| Cross-domain verification | - | Respect expertise boundaries |
| Consensus verification | Three-Layer (L3 social) | Institutional grounding |

---

## Quick Reference Checklist

**Before accepting any claim:**

```
[ ] Source credibility checked (Pattern 1)
[ ] Multiple sources confirm (Pattern 2)
[ ] Primary source verified (Pattern 3)
[ ] Bias evaluated (Pattern 4)
[ ] Timeliness confirmed (Pattern 5)
[ ] Domain expertise appropriate (Pattern 7)
[ ] Consensus assessed if applicable (Pattern 8)
[ ] Confidence level assigned
[ ] Claim accepted/rejected based on evidence
```

---

**Version:** 1.0
**Status:** Production
**Recommended for:** All fact-checking, research, and information verification tasks
