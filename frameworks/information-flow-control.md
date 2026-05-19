# Information Flow Control Framework

## Overview

Information flow control manages **what knowledge moves where** and **when**. In a distributed system with multiple agents, repositories, and operational layers, uncontrolled information flow creates security risks, privacy violations, and operational confusion.

This framework implements **L2 (Empathic/Safety) layer controls** on information movement.

## Core Principle

> **Default Deny, Explicit Allow**
>
> Information stays where it is unless there's an explicit reason and authorization for it to move.

## The Problem: Promiscuous Information Flow

**Without controls:**
- Private client data leaks into public repositories
- Proprietary prompts end up in git history
- Agent internal state becomes visible to users
- Debugging information exposes security vulnerabilities
- Personal information appears in logs and commits

**Example failure:**
```
Agent debugging: "Client John Smith (john@acme.com) reported bug in payment flow..."
→ Committed to public repo
→ Now in git history forever
→ Privacy violation
```

## Three-Layer Information Classification

### Layer 1: Public (World-Readable)
- **Content:** Documentation, schemas, examples, frameworks, patterns
- **Repositories:** `*-public-*` repos (identity, knowledge, world, system)
- **Movement:** Can flow anywhere
- **Protection:** None needed (already public)

**Examples:**
- JSON schemas for entities
- Verification patterns
- Constitutional reasoning framework
- Template files
- Public documentation

### Layer 2: Internal (Organization)
- **Content:** Client configurations, workflows, agent state, operational patterns
- **Repositories:** `*-private` repos (identity-private, knowledge-private, world-private, system-private)
- **Movement:** Within organization only; never to public repos
- **Protection:** Anonymization required before any cross-boundary movement

**Examples:**
- Client provisioning records
- Agent reflection logs
- Worktree allocation state
- PR dependency tracking
- Operational procedures

### Layer 3: Secret (Restricted)
- **Content:** API keys, credentials, personal data, proprietary algorithms
- **Repositories:** `*-machine` repos (identity-machine, system-machine, registry-machine)
- **Movement:** Never moves; accessed in-place only
- **Protection:** Encryption at rest, access logging

**Examples:**
- API keys and tokens
- Client contact information
- Billing data
- Security credentials
- Proprietary prompts

## Information Flow Rules

### Rule 1: Public ← Internal (Extraction)
**Direction:** Internal/Private → Public

**When:** Creating documentation, examples, schemas from private operational knowledge

**Process:**
1. **Identify:** What knowledge should be public?
2. **Anonymize:** Remove all identifying information
3. **Generalize:** Convert specific cases to general patterns
4. **Review:** Check for accidental exposure
5. **Extract:** Move to public repo

**Example:**
```
PRIVATE: "Client Acme Corp uses custom auth flow with Okta SSO"
   ↓ (anonymization + generalization)
PUBLIC: "Organizations may implement SSO authentication using SAML or OAuth providers"
```

**Checklist:**
- [ ] No client names or identifiers
- [ ] No personal information
- [ ] No proprietary details
- [ ] No security-sensitive information
- [ ] No API keys or credentials
- [ ] No internal URLs or systems

### Rule 2: Internal ← Secret (Operational Use)
**Direction:** Secret/Machine → Internal/Private

**When:** Agents need credentials or secrets to operate

**Process:**
1. **Access in-place:** Read secrets from machine repos
2. **Use in memory:** Never write secrets to disk outside machine repos
3. **Log access:** Record what was accessed, when, by whom
4. **Never persist:** Don't save secrets in agent state or logs

**Anti-Pattern:**
```python
# WRONG
api_key = read_secret("openai_key")
save_to_config({"api_key": api_key})  # ❌ Now in private repo

# RIGHT
api_key = read_secret("openai_key")
client = OpenAI(api_key=api_key)  # ✅ Used in memory only
```

### Rule 3: No Secret → Public Ever
**Direction:** Secret/Machine → Public

**Status:** **PROHIBITED**

**Why:** Once information is public, it cannot be un-public.

**Enforcement:**
- Pre-commit hooks scan for secrets
- CI/CD blocks commits with secret patterns
- Manual review required for any cross-boundary movement

**Detection patterns:**
- API key formats (`sk-...`, `Bearer ...`)
- Email addresses
- Phone numbers
- Credit card numbers
- Personal names in specific contexts
- Internal URLs

### Rule 4: Anonymization for Cross-Layer Movement

**When moving from restricted to less restricted:**

**Anonymization techniques:**

1. **Remove identifiers:**
   - Names → Role descriptions ("client", "user", "organization")
   - Email addresses → Generic placeholders
   - URLs → Generic examples (example.com)
   - Dates → Relative times ("recently", "last month")

2. **Generalize specifics:**
   - "Client uses PostgreSQL 14.2" → "Database systems"
   - "Bug in payment-processor.py:127" → "Payment processing logic"
   - "John's MacBook" → "Development machine"

3. **Aggregate data:**
   - Individual client behavior → Statistical patterns
   - Specific error messages → Error categories
   - Precise timestamps → Time ranges

4. **Replace with equivalents:**
   - Real company names → Example names (Acme Corp)
   - Real people → Personas (Alice, Bob)
   - Actual systems → Reference architectures

**Example transformation:**
```
BEFORE (Internal):
"Client Acme Corp (ID: client-001) reported that user jsmith@acme.com
received error 'null pointer exception' in checkout flow when using
coupon code SAVE20 on 2026-05-15 at 14:32:17 UTC. Investigation showed
bug in src/payments/coupon-validator.ts line 89."

AFTER (Public):
"Organizations may encounter validation errors during checkout when
applying discount codes. Common causes include null reference errors
in coupon validation logic. Recommended: add defensive null checks
before accessing coupon properties."
```

## Information Flow Patterns

### Pattern 1: Read-Only Cross-Layer Access
**Scenario:** Public agent needs context from private repo

**Solution:**
```
Agent (public context)
    ↓ (read-only)
Private repo (in-place access)
    ↓ (anonymized results only)
Agent (continues with anonymized context)
```

**Example:** Agent reads reflection log to learn from past mistakes, but never writes reflection log content into commit messages or public documentation.

### Pattern 2: One-Way Extract-and-Publish
**Scenario:** Operational pattern should become documentation

**Solution:**
```
Private repo (pattern emerges from operation)
    ↓ (manual review)
Anonymization process
    ↓ (creates)
Public repo (documentation)
```

**Example:** After solving similar problems multiple times, extract the pattern into a public framework document.

### Pattern 3: Secret Reference (No Flow)
**Scenario:** Agent needs API key

**Solution:**
```
Agent: "I need OpenAI key"
    ↓ (access request)
Secret store (identity-machine or system-machine)
    ↓ (returns in-memory only)
Agent: uses key, never persists
```

**Example:** Agent reads API key from machine repo, uses it for API call, key never appears in logs or state.

### Pattern 4: Sanitized Logging
**Scenario:** Need to log operations for debugging

**Solution:**
```
Operation occurs (may involve secrets)
    ↓ (before logging)
Sanitization filter
    ↓ (removes secrets, PII)
Log file (safe to persist)
```

**Example:**
```
BEFORE FILTER: "API call to https://api.openai.com with key sk-proj-abc123..."
AFTER FILTER: "API call to https://api.openai.com with key [REDACTED]"
```

## Implementation

### Pre-Commit Hook (Git)
```bash
#!/bin/bash
# .git/hooks/pre-commit

# Detect secrets in staged files
if git diff --cached | grep -E '(sk-[a-zA-Z0-9]{32,}|Bearer [a-zA-Z0-9]+)'; then
  echo "❌ SECRET DETECTED in staged changes"
  echo "Commit blocked. Remove secrets before committing."
  exit 1
fi

# Detect email addresses (except generic examples)
if git diff --cached | grep -E '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' | grep -v 'example.com'; then
  echo "⚠️  EMAIL ADDRESS detected in staged changes"
  echo "Verify this should be committed. Use example.com for examples."
  exit 1
fi

echo "✅ No secrets detected"
exit 0
```

### Anonymization Function (Python)
```python
import re
from typing import str

def anonymize_for_public(text: str) -> str:
    """
    Anonymize text for public consumption.
    Removes emails, API keys, personal names in context.
    """
    # Remove API keys
    text = re.sub(r'sk-[a-zA-Z0-9]{32,}', '[API_KEY_REDACTED]', text)
    text = re.sub(r'Bearer [a-zA-Z0-9]+', '[TOKEN_REDACTED]', text)

    # Remove email addresses (except example.com)
    text = re.sub(
        r'\b[A-Za-z0-9._%+-]+@(?!example\.com)[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',
        '[EMAIL_REDACTED]',
        text
    )

    # Remove URLs except generic examples
    text = re.sub(
        r'https?://(?!example\.com)([a-zA-Z0-9.-]+)',
        'https://[REDACTED]',
        text
    )

    # Remove phone numbers (North American format)
    text = re.sub(
        r'\b\d{3}[-.]?\d{3}[-.]?\d{4}\b',
        '[PHONE_REDACTED]',
        text
    )

    return text
```

### Access Control (Conceptual)
```python
class InformationFlowController:
    """
    Controls information flow between layers.
    """

    LAYERS = {
        'public': 0,
        'internal': 1,
        'secret': 2
    }

    def can_flow(self, source_layer: str, target_layer: str) -> bool:
        """
        Check if information can flow from source to target.

        Rule: Information can only flow to same or more restricted layers.
        """
        source_level = self.LAYERS[source_layer]
        target_level = self.LAYERS[target_layer]

        # Can flow to same level
        if source_level == target_level:
            return True

        # Can flow to more restricted (higher number)
        if target_level > source_level:
            return True

        # Cannot flow to less restricted
        # (requires manual anonymization process)
        return False

    def require_anonymization(self, source_layer: str, target_layer: str) -> bool:
        """
        Check if anonymization is required for this flow.
        """
        source_level = self.LAYERS[source_layer]
        target_level = self.LAYERS[target_layer]

        # Anonymization required when flowing to less restricted layer
        return target_level < source_level
```

## L2 Integration

This framework is an **L2 (Empathic/Safety) layer implementation**:

**L2 Purpose:** Prevent harm before it occurs

**How this framework serves L2:**
1. **Prevents privacy violations** (personal data exposure)
2. **Prevents security incidents** (credential leakage)
3. **Prevents compliance failures** (GDPR, data protection laws)
4. **Prevents reputation damage** (client confidentiality breaches)

**Constitutional Check:**
```
Agent: "I should document this client issue in the public repo"
    ↓
L1 (Rational): Does documenting help future users? YES
    ↓
L2 (Empathic): Does this documentation expose client data? CHECK
    ↓
Information Flow Control: Client data detected. Anonymize first.
    ↓
L2 Brake: HOLD. Anonymize before proceeding.
```

## Common Violations and Fixes

### Violation 1: Client names in commit messages
```
# ❌ WRONG
git commit -m "Fix bug for Acme Corp in auth flow"

# ✅ RIGHT
git commit -m "Fix authentication flow edge case"
```

### Violation 2: Email addresses in examples
```
# ❌ WRONG
Example: john.smith@acmecorp.com

# ✅ RIGHT
Example: user@example.com
```

### Violation 3: API keys in config files
```
# ❌ WRONG
# config.yaml
openai_api_key: "sk-proj-abc123..."

# ✅ RIGHT
# config.yaml
openai_api_key: ${OPENAI_API_KEY}  # Read from environment

# Store actual key in:
# jengo-system-machine/secrets/api-keys.enc
```

### Violation 4: Detailed error messages with internal paths
```
# ❌ WRONG
Error in /home/jengo/clients/acme/src/payment.py line 127

# ✅ RIGHT
Error in payment processing module
```

## Audit and Monitoring

### Access Logging
```
{
  "timestamp": "2026-05-19T14:23:11Z",
  "agent": "agent-003",
  "action": "read_secret",
  "resource": "system-machine/secrets/openai-key",
  "outcome": "success"
}
```

### Flow Violation Detection
```
{
  "timestamp": "2026-05-19T14:25:03Z",
  "violation_type": "unauthorized_flow",
  "source_layer": "secret",
  "target_layer": "public",
  "resource": "api-key",
  "action_taken": "blocked",
  "agent": "agent-003"
}
```

### Regular Audits
- **Weekly:** Review access logs for unusual patterns
- **Monthly:** Scan public repos for accidental secret exposure
- **Quarterly:** Review anonymization procedures for effectiveness

## Summary

**Key Takeaways:**
1. Information has layers: Public, Internal, Secret
2. Default deny: Information stays put unless explicitly allowed
3. Flowing to less restricted layers requires anonymization
4. Never flow secrets to public
5. Pre-commit hooks and sanitization prevent violations
6. This is an L2 (safety) framework: prevents harm before it occurs

**Enforcement:**
- Technical (pre-commit hooks, sanitization functions)
- Procedural (review checklist, audit logs)
- Constitutional (L2 brake stops unsafe information flow)

**When in doubt:**
> If you're not sure whether information should flow, **don't flow it**.
> Ask for review first.
