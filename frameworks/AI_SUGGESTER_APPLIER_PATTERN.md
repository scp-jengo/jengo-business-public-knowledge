# AI Suggester-Applier Allowlist Pattern

**Type:** Security Pattern / Architecture Pattern
**Domain:** LLM Integration, API Design, Trust Boundaries
**Audience:** Engineering Teams, System Architects, Security Engineers
**Status:** Production-validated
**Last Updated:** 2026-05-25

---

## Executive Summary

When integrating Large Language Models (LLMs) into systems with **closed action sets** (databases, APIs, file systems), a three-layer defense pattern is required to ensure **behavioral honesty** — that what the system tells users happened actually matches what was executed.

**Core Problem:** LLMs will claim actions are "auto-fixable" or "applied" even when the underlying system cannot execute them. This creates invisible failures where users see success indicators (green checkmarks, "Applied" status) for work that never occurred.

**Solution:** Implement constraints at **all three layers**: prompt, server gate, and frontend reconciliation.

---

## Problem Statement

### Failure Mode

**Scenario:**
1. LLM analyzes a problem and suggests fixes
2. LLM marks suggestions as `is_automatable: true` or `status: Applied`
3. System attempts to apply fixes via deterministic applier function
4. Applier has a **closed set** of supported operations (e.g., can edit field A, B, C but not D)
5. LLM suggested fix for field D → applier throws exception
6. Exception caught in batch loop → silently logged
7. User sees "Applied" status → trusts the work was done
8. **Reality:** Nothing changed in the actual system

**Why This Happens:**
- LLMs are trained to be helpful (RLHF optimization)
- LLMs say "yes, this is fixable" more often than systems can deliver
- Batch error handling swallows individual failures
- Frontend optimistically marks success based on HTTP 200, not actual state change

**Detection:**
- Often invisible until external verification (e.g., checking rendered output)
- Internal metrics look perfect (100% success rate)
- User only notices when they manually verify work was NOT done

---

## The Pattern: Three-Layer Defense

```
┌─────────────────────────────────────────────────────────┐
│ Layer 1: PROMPT-LEVEL CONSTRAINT                        │
│ "You may only suggest fixes for fields: A, B, C"        │
│ ↓                                                        │
│ LLM Output: { field: "D", is_automatable: true }        │
│                                                          │
│ ⚠️ FAILS: LLMs ignore constraints often enough to       │
│           require defense-in-depth                       │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ Layer 2: SERVER-SIDE ALLOWLIST GATE                     │
│ allowlist = ["A", "B", "C"]                              │
│ if suggestion.field not in allowlist:                    │
│     suggestion.is_automatable = false                    │
│     suggestion.reason = "Field not supported"            │
│ ↓                                                        │
│ ✅ CATCHES: Downgrades unsupported suggestions           │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ Layer 3: APPLIER DEFENSE-IN-DEPTH                       │
│ def apply(suggestion):                                   │
│     if suggestion.field not in SUPPORTED_FIELDS:         │
│         raise UnsupportedFieldError                      │
│     # ... actual application logic                       │
│ ↓                                                        │
│ ✅ FINAL SAFEGUARD: Throws if gate failed               │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ Layer 4: FRONTEND RECONCILIATION                        │
│ Backend returns: audit_log = [successfully_applied_ids] │
│ Frontend marks "Applied" ONLY for IDs in audit_log       │
│ ↓                                                        │
│ ✅ USER SEES: Accurate status based on actual outcome   │
└─────────────────────────────────────────────────────────┘
```

---

## Core Concepts

### 1. Suggester Step
The LLM call that analyzes problems and proposes solutions.

**Outputs:**
```json
{
  "suggestions": [
    {
      "id": "S1",
      "description": "Add featured image",
      "field_name": "featured_image",
      "is_automatable": true,
      "auto_fix": {
        "field": "featured_image",
        "value": "https://example.com/image.jpg"
      }
    }
  ]
}
```

**Issue:** LLM may set `is_automatable: true` for fields the applier doesn't support.

### 2. Applier Step
The deterministic code that writes to database/API/filesystem.

**Has finite supported operations:**
```python
SUPPORTED_FIELDS = ["title", "description", "content"]

def apply_suggestion(suggestion):
    if suggestion.field not in SUPPORTED_FIELDS:
        raise UnsupportedFieldError(f"Cannot apply to {suggestion.field}")

    # ... execute write operation
```

**Issue:** If called with unsupported field, throws exception.

### 3. Allowlist Gate
Pure function that validates suggestions **before** applier execution.

**Location:** Server-side (not client-side, not LLM-level only)

**Logic:**
```python
def allowlist_gate(suggestions, supported_fields):
    validated = []
    for suggestion in suggestions:
        if suggestion.field in supported_fields:
            validated.append(suggestion)
        else:
            # Downgrade to manual
            suggestion.is_automatable = False
            suggestion.reason = f"Field '{suggestion.field}' requires manual review"
            validated.append(suggestion)
    return validated
```

**Purpose:** Prevent unsupported suggestions from reaching applier.

### 4. Optimistic Applied Pattern
UI pattern where frontend immediately shows "Applied" before server confirms.

**Without reconciliation:**
- Frontend: "I sent it → must be applied" (wrong assumption)
- Backend: Exception in batch loop → silently logged
- User sees: ✅ Applied
- Reality: ❌ Not applied

**With reconciliation:**
```javascript
// Frontend receives response
const response = await applyFixes(suggestions);

// Backend returns ONLY successfully applied IDs
const { applied_ids } = response;

// Frontend marks as Applied ONLY those in applied_ids
suggestions.forEach(s => {
  s.status = applied_ids.includes(s.id) ? "Applied" : "Pending";
});
```

### 5. Fudging
LLM picks an **in-allowlist** field but writes an **out-of-allowlist** fix into it.

**Example:**
- Suggestion: "Add featured image"
- LLM knows `featured_image` field not supported
- LLM picks `content` field (supported) and writes HTML mentioning images
- Technically "applied" but doesn't actually add featured image
- **Hardest to detect** — requires semantic validation

**Mitigation:** Topic-keyword detection on top of field validation
```python
FIELD_TOPIC_MAP = {
    "featured_image": ["image", "picture", "photo", "visual"],
    "author": ["author", "writer", "by"],
}

def detect_fudging(suggestion):
    field_topics = FIELD_TOPIC_MAP.get(suggestion.intended_field, [])
    if any(topic in suggestion.description.lower() for topic in field_topics):
        if suggestion.field != suggestion.intended_field:
            return True  # Fudging detected
    return False
```

---

## Architecture

### Minimal Honest Pipeline

```
User Request
    ↓
Suggester (LLM)
    ↓ JSON: [{field, is_automatable, auto_fix}, ...]
Allowlist Gate (server-side)
    ↓ Downgrades unsupported items: is_automatable → false
Applier (executes writes)
    ↓ Defense-in-depth: throws if unsupported (should never be reached)
Audit Log (records ONLY successful applies)
    ↓ Returns: {applied_ids: ["S1", "S3"], failed_ids: ["S2"]}
Frontend Reconciliation
    ↓ Marks "Applied" ONLY for IDs in applied_ids
User sees accurate status
```

### Failure Modes When Layers Missing

| Missing Layer | Failure Mode |
|---------------|--------------|
| **No Prompt Constraint** | LLM suggests anything, including impossible operations |
| **No Allowlist Gate** | Unsupported suggestions reach applier → exceptions → silent failures |
| **No Applier Defense** | If gate has bug, bad data reaches database → corruption |
| **No Frontend Reconciliation** | UI shows "Applied" based on HTTP 200, not actual state → user trusts false status |

---

## Implementation Checklist

### Pre-Development

- [ ] **Document closed action set:** What operations can the applier ACTUALLY perform?
- [ ] **Enumerate supported fields:** Which database fields / API endpoints are writable?
- [ ] **Define topics/categories:** What semantic categories do fields represent?
- [ ] **Architect for failure:** What happens when LLM suggests unsupported action?

### Development

**Layer 1: Prompt-Level**
- [ ] System prompt explicitly lists supported fields
- [ ] Prompt includes examples of valid suggestions
- [ ] Prompt warns against suggesting unsupported operations

**Layer 2: Server-Side Gate**
- [ ] Implement `allowlist_gate(suggestions, supported_fields)` function
- [ ] Downgrade `is_automatable = false` for unsupported items
- [ ] Add `reason` field explaining why not automatable
- [ ] Log all downgrades for monitoring

**Layer 3: Applier Defense**
- [ ] Implement field validation in applier
- [ ] Throw explicit exception for unsupported fields
- [ ] Do NOT catch these exceptions silently
- [ ] Log all exceptions with full context

**Layer 4: Frontend Reconciliation**
- [ ] Backend returns `applied_ids` list (not just HTTP status)
- [ ] Frontend updates UI based on `applied_ids`, not optimistic assumption
- [ ] Display error reasons for failed/downgraded items
- [ ] Provide manual review option for downgraded items

### Testing

- [ ] **Happy path:** Supported field suggestion → applies successfully
- [ ] **Unsupported field:** LLM suggests field D (not in allowlist) → downgraded → not applied
- [ ] **Fudging detection:** LLM suggests "image" topic with "content" field → flagged
- [ ] **Batch error:** One suggestion fails → others succeed → status accurate for each
- [ ] **Optimistic UI:** Frontend shows pending → backend responds → UI reconciles correctly

### Monitoring

- [ ] **Downgrade rate:** % of suggestions downgraded by gate (should be low, indicates prompt ineffective)
- [ ] **Applier exceptions:** # of exceptions thrown (should be zero, indicates gate working)
- [ ] **Reconciliation mismatches:** Optimistic status ≠ actual status (should be zero)
- [ ] **User-reported discrepancies:** Users notice "Applied" but output unchanged (RED FLAG)

---

## Real-World Examples

### Example 1: Content Management System

**Closed Action Set:** `title`, `description`, `content`, `tags`

**LLM Suggestion:**
```json
{
  "description": "Add featured image to improve engagement",
  "field": "featured_image",
  "is_automatable": true
}
```

**Without Gate:** Applier throws `UnsupportedFieldError` → caught by batch loop → user sees "Applied" → featured image not actually added

**With Gate:**
```json
{
  "description": "Add featured image to improve engagement",
  "field": "featured_image",
  "is_automatable": false,
  "reason": "Field 'featured_image' requires manual review (not in supported set)"
}
```
User sees "Manual Review Required" → accurate status

### Example 2: Database Record Updater

**Closed Action Set:** Update `status`, `priority`, `assigned_to`

**LLM Fudging Attempt:**
```json
{
  "description": "Delete spam record",
  "field": "status",  // Supported field
  "auto_fix": {"status": "deleted"}  // But "delete" is not a status value
}
```

**Detection:**
```python
ALLOWED_STATUS_VALUES = ["open", "in_progress", "closed"]

def validate_fix(suggestion):
    if suggestion.field == "status":
        if suggestion.auto_fix.value not in ALLOWED_STATUS_VALUES:
            return False, "Status value not in allowed set"
    return True, None
```

---

## Common Pitfalls

### Pitfall 1: "Prompt is enough"
**Assumption:** LLM will respect field constraints in system prompt

**Reality:** LLMs ignore constraints often enough (5-15% of cases) to require server-side enforcement

**Fix:** Always implement Layer 2 (server gate)

### Pitfall 2: "HTTP 200 = Success"
**Assumption:** If API returns 200 OK, the operation succeeded

**Reality:** Batch operations may return 200 with partial failures silently logged

**Fix:** Return explicit `applied_ids` list, reconcile on frontend

### Pitfall 3: "Exception = User notification"
**Assumption:** Applier exception will automatically notify user

**Reality:** Exceptions in batch loops often caught by outer try/catch → logged but not surfaced

**Fix:** Audit log pattern — only mark success if audit log entry created

### Pitfall 4: "Field name validation is sufficient"
**Assumption:** Checking `field in SUPPORTED_FIELDS` catches all issues

**Reality:** LLM can pick valid field but suggest invalid operation (fudging)

**Fix:** Add semantic/topic validation for high-stakes operations

---

## Security Considerations

### Trust Boundary

**Never trust LLM output without validation.**

```
┌──────────────────────────────────┐
│  UNTRUSTED ZONE                  │
│  - User input                    │
│  - LLM output                    │
│  - Client-side validation        │
└──────────────────────────────────┘
           ↓
┌──────────────────────────────────┐
│  TRUST BOUNDARY (Server-side)    │
│  - Allowlist gate                │
│  - Input sanitization            │
│  - Authorization checks          │
└──────────────────────────────────┘
           ↓
┌──────────────────────────────────┐
│  TRUSTED ZONE                    │
│  - Database writes               │
│  - External API calls            │
│  - File system operations        │
└──────────────────────────────────┘
```

**Principle:** LLM output crosses trust boundary. Validate at boundary, not after.

### Injection Risks

**SQL Injection:**
```python
# BAD: Direct LLM output to SQL
query = f"UPDATE posts SET {suggestion.field} = '{suggestion.value}'"

# GOOD: Allowlist + parameterized queries
if suggestion.field not in ALLOWED_FIELDS:
    raise ValueError("Unsupported field")
query = "UPDATE posts SET ? = ? WHERE id = ?"
execute(query, (suggestion.field, suggestion.value, post_id))
```

**Command Injection:**
```python
# BAD: LLM output to shell command
os.system(f"convert image.jpg -resize {suggestion.size}")

# GOOD: Allowlist + safe API
ALLOWED_SIZES = ["small", "medium", "large"]
if suggestion.size not in ALLOWED_SIZES:
    raise ValueError("Invalid size")
resize_image(image_path, SIZE_MAP[suggestion.size])
```

---

## Integration with Other Patterns

### Pattern: Human-in-the-Loop
- LLM suggests (no execution authority)
- System validates and presents to human
- Human approves or rejects
- System executes approved actions only

**Allowlist gate** becomes **allowlist + presentation**:
```python
def gate_and_present(suggestions):
    auto_approved = []
    needs_review = []

    for s in suggestions:
        if s.field in AUTO_APPROVE_FIELDS and validate(s):
            auto_approved.append(s)
        else:
            needs_review.append(s)

    return {
        "auto_approved": auto_approved,
        "needs_review": needs_review
    }
```

### Pattern: Audit Trail
- All suggestions logged (approved and rejected)
- All applications logged with timestamp, user, outcome
- Enables:
  - Debugging ("why wasn't this applied?")
  - Compliance ("who changed this field?")
  - Learning ("which suggestions get rejected most?")

```python
audit_log.record({
    "timestamp": now(),
    "suggestion_id": s.id,
    "action": "downgrade",
    "reason": "field_not_in_allowlist",
    "field": s.field,
    "supported_fields": SUPPORTED_FIELDS
})
```

---

## Metrics and Monitoring

### Health Indicators

**Healthy System:**
- Downgrade rate: <5% (prompt is effective)
- Applier exceptions: 0 (gate is working)
- User reports: 0 (status matches reality)

**Warning Signs:**
- Downgrade rate: 15-30% → Prompt needs improvement
- Applier exceptions: >0 → Gate has bugs
- User reports: >0 → Frontend reconciliation failing

### Dashboards

**Suggester Health:**
- Total suggestions
- Auto-fixable rate (by LLM)
- Downgrade rate (by gate)
- Actual application rate

**Applier Health:**
- Total apply attempts
- Success rate
- Exception rate
- Exception types

**User Experience:**
- "Applied" shown to user
- Actually applied
- Discrepancy rate ⚠️

---

## Conclusion

**The Problem:** LLMs over-promise, systems under-deliver, users see false success.

**The Solution:** Three-layer defense (prompt + gate + applier) + frontend reconciliation.

**The Principle:** Trust but verify. Every layer validates independently.

**Business Impact:** User trust depends on behavioral honesty. When the system says "Applied," it must be true.

---

## Implementation Resources

### Code Templates

**Server-side gate:**
```python
def allowlist_gate(suggestions, supported_fields, topic_map=None):
    validated = []
    for s in suggestions:
        if s.field not in supported_fields:
            s.is_automatable = False
            s.reason = f"Field '{s.field}' not in supported set"
        elif topic_map and detect_fudging(s, topic_map):
            s.is_automatable = False
            s.reason = f"Topic mismatch detected (fudging)"
        validated.append(s)
    return validated
```

**Applier defense:**
```python
def apply_suggestion(suggestion, supported_fields):
    if suggestion.field not in supported_fields:
        raise UnsupportedFieldError(
            f"Field '{suggestion.field}' cannot be modified. "
            f"Supported: {supported_fields}"
        )

    # Actual application logic
    db.update(field=suggestion.field, value=suggestion.value)

    # Record in audit log
    audit_log.record(suggestion.id, "applied", timestamp=now())

    return {"id": suggestion.id, "status": "applied"}
```

**Frontend reconciliation:**
```javascript
async function applyFixes(suggestions) {
  const response = await api.post("/apply", { suggestions });
  const { applied_ids, failed_ids } = response.data;

  // Update UI based on actual outcome
  suggestions.forEach(s => {
    if (applied_ids.includes(s.id)) {
      s.status = "Applied";
      s.statusClass = "success";
    } else if (failed_ids.includes(s.id)) {
      s.status = "Failed";
      s.statusClass = "error";
    } else {
      s.status = "Pending";
      s.statusClass = "warning";
    }
  });

  return suggestions;
}
```

---

**Pattern Status:** Production-validated
**Recommended Review:** When adding new LLM-integrated features
**Related Patterns:** Human-in-the-Loop, Audit Trail, Defense-in-Depth

---

*This pattern is part of the Jengo Business Public Knowledge repository. Licensed under MIT for business use. Contributions welcome.*
