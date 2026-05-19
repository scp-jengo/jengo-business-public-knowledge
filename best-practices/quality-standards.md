# Knowledge Quality Standards

## Overview

Not all knowledge is created equal. High-quality knowledge is actionable, evidence-based, well-structured, and maintainable. Low-quality knowledge creates noise, confusion, and wasted effort.

This document defines **quality standards for knowledge contributions**.

## Quality Dimensions

### 1. Accuracy
**Definition:** Knowledge correctly describes reality

**Standards:**
- ✅ **HIGH:** Verified through multiple applications, evidence documented
- ⚠️ **MEDIUM:** Applied successfully but limited evidence
- ❌ **LOW:** Theoretical or speculative, no empirical validation

**How to achieve:**
- Document actual outcomes when applied
- Include case studies
- Cite empirical data
- Link to external validation (research, industry standards)

**Anti-pattern:**
```markdown
# ❌ LOW ACCURACY
Pattern: "Use microservices for better scalability"
[No evidence, no context, oversimplified]

# ✅ HIGH ACCURACY
Pattern: "Microservices for bounded contexts"
Evidence:
- Improved deployment frequency: 2x in Q1 2026 (3 teams)
- Reduced blast radius: failures contained to single service
- Trade-off: Increased operational complexity, monitoring overhead
Case study: Payment service extraction (anonymized)
When to use: Team size >8, distinct deployment cycles needed
When NOT to use: Small team, shared database, tight coupling
```

### 2. Actionability
**Definition:** Knowledge provides clear guidance on what to do

**Standards:**
- ✅ **HIGH:** Step-by-step process, decision criteria, examples
- ⚠️ **MEDIUM:** General approach described, some details missing
- ❌ **LOW:** Vague principles with no practical guidance

**How to achieve:**
- Include concrete steps
- Provide decision trees or matrices
- Show before/after examples
- Include code snippets or commands

**Anti-pattern:**
```markdown
# ❌ LOW ACTIONABILITY
"Practice good security"

# ✅ HIGH ACTIONABILITY
## Security Checklist for API Endpoints

1. **Input Validation**
   ```python
   from pydantic import BaseModel, validator

   class PaymentRequest(BaseModel):
       amount: float
       currency: str

       @validator('amount')
       def amount_positive(cls, v):
           if v <= 0:
               raise ValueError('Amount must be positive')
           return v
   ```

2. **Authentication**
   - Verify JWT signature: `jwt.decode(token, public_key, algorithms=['RS256'])`
   - Check expiration: `payload['exp'] > current_time()`

3. **Authorization**
   - Load user permissions from: `auth_service.get_permissions(user_id)`
   - Check required permission: `'payment:create' in user.permissions`

4. **Rate Limiting**
   - Apply per-user limit: 10 requests/minute
   - Implementation: Redis with sliding window
   ```
```

### 3. Completeness
**Definition:** Knowledge includes all necessary context and information

**Standards:**
- ✅ **HIGH:** Context, application, constraints, alternatives, examples
- ⚠️ **MEDIUM:** Core information present, some context missing
- ❌ **LOW:** Incomplete, missing critical context or examples

**Completeness checklist:**
- [ ] Problem statement (what does this solve?)
- [ ] Solution (how to solve it?)
- [ ] When to use (context boundaries)
- [ ] When NOT to use (anti-indications)
- [ ] Examples (concrete illustrations)
- [ ] Evidence (empirical support)
- [ ] Related knowledge (links)
- [ ] Layer identification (which layer?)

**Anti-pattern:**
```markdown
# ❌ INCOMPLETE
Pattern: "Use caching"

# ✅ COMPLETE
## Pattern: Redis Caching for API Responses

### Problem
API responses are compute-intensive, causing high latency (>500ms)
and database load.

### Solution
Cache computed responses in Redis with TTL-based expiration.

### When to Use
- Read-heavy workload (read:write ratio >10:1)
- Expensive computation (>100ms)
- Acceptable staleness (data changes <1x/minute)
- Bounded cache size (predictable key space)

### When NOT to Use
- Real-time data requirements (staleness unacceptable)
- Unbounded key space (memory exhaustion risk)
- Write-heavy workload (cache constantly invalidated)
- Simple queries (<10ms) where cache overhead dominates

### Implementation
```python
import redis
import json
from functools import wraps

cache = redis.Redis(host='localhost', port=6379, db=0)

def cache_response(ttl=300):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            key = f"{func.__name__}:{args}:{kwargs}"
            cached = cache.get(key)
            if cached:
                return json.loads(cached)
            result = func(*args, **kwargs)
            cache.setex(key, ttl, json.dumps(result))
            return result
        return wrapper
    return decorator

@cache_response(ttl=300)
def get_user_stats(user_id):
    # Expensive computation
    return compute_stats(user_id)
```

### Evidence
- Latency reduced: 500ms → 50ms (10x improvement)
- Database load reduced: 1000 queries/sec → 100 queries/sec
- Cache hit rate: 92% (Q1 2026, 3 services)

### Trade-offs
- **Benefit:** Reduced latency, lower database load
- **Cost:** Memory usage (plan for 1GB per 100k keys), cache invalidation complexity

### Related
- [Cache Invalidation Patterns](./cache-invalidation.md)
- [Redis Configuration](../../device/redis-config.md)
- Foundation: [Performance Optimization](../frameworks/performance.md)
```

### 4. Clarity
**Definition:** Knowledge is easy to understand and unambiguous

**Standards:**
- ✅ **HIGH:** Clear language, well-structured, scannable
- ⚠️ **MEDIUM:** Generally understandable, some ambiguity
- ❌ **LOW:** Confusing, jargon-heavy, hard to parse

**Clarity guidelines:**
- Use active voice ("Validate inputs" not "Inputs should be validated")
- Define jargon on first use
- Use headings, bullets, code blocks for structure
- Provide examples to clarify abstract concepts
- One concept per paragraph

**Anti-pattern:**
```markdown
# ❌ LOW CLARITY
"The utilization of the observer pattern facilitates the implementation
of a publish-subscribe architecture whereby subscribers are notified of
events through a callback mechanism that is registered with the publisher,
thereby decoupling the publisher from knowledge of specific subscribers."

# ✅ HIGH CLARITY
## Observer Pattern for Event Notification

**Problem:** Component A needs to notify components B, C, D when an event occurs,
but shouldn't know about B, C, D directly (tight coupling).

**Solution:** Observer pattern

```python
class EventPublisher:
    def __init__(self):
        self._subscribers = []

    def subscribe(self, callback):
        self._subscribers.append(callback)

    def publish(self, event):
        for callback in self._subscribers:
            callback(event)

# Usage
publisher = EventPublisher()
publisher.subscribe(lambda event: print(f"B received: {event}"))
publisher.subscribe(lambda event: print(f"C received: {event}"))
publisher.publish("user_created")
```

**Benefit:** Publisher doesn't know about B or C. Add new subscribers without changing publisher.
```

### 5. Maintainability
**Definition:** Knowledge can be updated and kept current over time

**Standards:**
- ✅ **HIGH:** Version tracked, dependencies clear, evolution documented
- ⚠️ **MEDIUM:** Some tracking, dependencies implicit
- ❌ **LOW:** No versioning, unclear dependencies, static

**Maintainability practices:**
- Include `metadata.created` and `metadata.updated` dates
- Document revision history
- Link to dependencies (other knowledge items)
- Mark deprecated knowledge with replacement
- Use semantic status (draft, review, stable, deprecated)

**Example:**
```markdown
# Pattern: Authentication Flow

## Metadata
- **Created:** 2024-06-01
- **Updated:** 2026-05-15
- **Status:** stable
- **Layer:** department (backend)

## Dependencies
- Foundation: [Zero-Trust Security](../frameworks/zero-trust.md)
- Organization: [Security Policy](../../organization/security-policy.md)
- Device: [JWT Library v2.3+](../../device/libraries/jwt.md)

## Revision History

### 2026-05-15: Added refresh token rotation
- Security improvement: refresh tokens now single-use
- Prevents token replay attacks
- Evidence: 0 token replay incidents since implementation

### 2025-08-22: Migrated to RS256
- Changed from HS256 (symmetric) to RS256 (asymmetric)
- Enables distributed verification
- Migration guide: [HS256-to-RS256.md](../migrations/hs256-to-rs256.md)

### 2024-06-01: Initial version
- Basic JWT authentication
- HS256 signing
```

### 6. Evidence-Based
**Definition:** Knowledge backed by empirical data or authoritative sources

**Standards:**
- ✅ **HIGH:** Empirical data, case studies, external validation
- ⚠️ **MEDIUM:** Some evidence, mostly experiential
- ❌ **LOW:** Opinion or speculation with no evidence

**Evidence types:**
1. **Empirical:** Quantitative data from actual application
2. **Theoretical:** Grounded in established principles or research
3. **Case Studies:** Detailed examples from real situations (anonymized)
4. **External Validation:** Industry standards, research papers, authoritative sources

**Example:**
```markdown
## Evidence

### Empirical
- Applied in 12 services (Q1-Q2 2026)
- Reduced cache-related bugs from 23 to 2 (91% reduction)
- Cache hit rate: avg 89% (range: 82%-94%)

### Theoretical
- Based on locality principle (temporal locality: recently accessed data likely to be accessed again)
- Aligned with CAP theorem trade-offs (sacrificing consistency for availability/partition tolerance)

### Case Studies
- **E-commerce product catalog** (anonymized):
  - Problem: Product detail API 500ms latency
  - Solution: Redis caching with 5-minute TTL
  - Result: Latency reduced to 45ms (11x improvement)
  - Trade-off: Product updates take up to 5 minutes to appear

### External Validation
- [Redis Best Practices](https://redis.io/docs/manual/patterns/)
- [Caching Strategies - AWS](https://aws.amazon.com/caching/best-practices/)
- [Research: Performance Impact of Caching](https://example.com/paper)
```

## Quality Levels

### Level 1: Minimum Viable (REQUIRED)
**All knowledge must meet this bar:**

```yaml
minimum_viable:
  required_sections:
    - title
    - content (problem + solution)
    - metadata (created, layer, status)
  required_attributes:
    - Clear problem statement
    - Actionable solution
    - At least one example
  information_quality:
    - Anonymized (no client data)
    - Layer-appropriate abstraction
    - No contradictions with existing knowledge
```

**Checklist:**
- [ ] Title clearly describes the knowledge
- [ ] Problem statement present
- [ ] Solution provided
- [ ] At least one example
- [ ] Metadata complete (created, layer, status)
- [ ] Anonymized (no sensitive information)
- [ ] Layer-appropriate abstraction level

### Level 2: Production Quality (TARGET)
**Knowledge should aim for this bar:**

```yaml
production_quality:
  required_sections:
    - title
    - problem
    - solution
    - when_to_use / when_not_to_use
    - examples (2+)
    - evidence
    - related_knowledge
    - metadata (full)
  required_attributes:
    - Clear decision criteria
    - Multiple examples
    - Some evidence (empirical or experiential)
    - Links to related knowledge
  quality_dimensions:
    - Accuracy: medium to high
    - Actionability: high
    - Completeness: medium to high
    - Clarity: high
    - Maintainability: medium to high
    - Evidence: medium to high
```

**Checklist:**
- [ ] All minimum viable requirements
- [ ] When to use / when NOT to use
- [ ] 2+ examples from different contexts
- [ ] Evidence section present
- [ ] Links to related knowledge
- [ ] Revision history started
- [ ] Dependencies documented

### Level 3: Exemplary (ASPIRATIONAL)
**Best-in-class knowledge:**

```yaml
exemplary_quality:
  additional_requirements:
    - Comprehensive evidence (empirical + theoretical + case studies)
    - Integration with constitutional framework
    - Cross-layer applicability demonstrated
    - Quantitative impact metrics
    - Edge cases identified and addressed
    - Evolution path suggested
  quality_dimensions:
    - All dimensions: high
    - External validation present
    - Long-term proven (6+ months)
```

**Characteristics:**
- Referenced by multiple other knowledge items
- Proven across diverse contexts
- Comprehensive evidence
- Clear integration with frameworks
- Actively maintained and updated
- Becomes foundation for other knowledge

## Quality Assurance Process

### Self-Review
**Before committing, review against checklist:**

```markdown
## Quality Self-Review

### Content
- [ ] Problem clearly stated
- [ ] Solution actionable and complete
- [ ] Examples provided (minimum 1, target 2+)
- [ ] Decision criteria included (when to use / when NOT)

### Evidence
- [ ] Evidence section present
- [ ] At least one evidence type (empirical/theoretical/case study/external)
- [ ] Quantitative data included if available

### Structure
- [ ] Follows knowledge-item schema
- [ ] Appropriate sections for type (pattern/framework/best-practice)
- [ ] Clear headings and formatting
- [ ] Scannable (bullets, code blocks, tables)

### Integration
- [ ] Layer identified correctly
- [ ] Links to related knowledge
- [ ] No contradictions with existing knowledge
- [ ] Integrates with relevant frameworks

### Information Control
- [ ] Anonymized (no client names, no PII)
- [ ] No secrets or credentials
- [ ] Appropriate abstraction for layer

### Metadata
- [ ] Created date
- [ ] Layer
- [ ] Status (draft/review/stable)
- [ ] Tags relevant

**Overall Quality Assessment:** [Minimum Viable | Production Quality | Exemplary]
```

### Peer Review
**When requesting review:**

```markdown
## Review Request

**Knowledge Item:** [link]
**Type:** [pattern | framework | best-practice | anti-pattern]
**Target Quality:** [minimum | production | exemplary]

**Specific Review Requests:**
- [ ] Accuracy: Is this correct? Any errors?
- [ ] Actionability: Can you follow this guidance?
- [ ] Clarity: Is anything confusing?
- [ ] Completeness: What's missing?

**Context:**
[Brief note on where this knowledge emerged from]
```

### Quality Audit
**Monthly review of knowledge base:**

1. **Identify quality issues:**
   - Outdated knowledge (not updated in >1 year)
   - Low-quality items (missing sections, no evidence)
   - Orphaned items (no references, not used)

2. **Triage:**
   - **Improve:** Worth keeping, needs quality upgrade
   - **Deprecate:** Obsolete or superseded
   - **Remove:** Wrong, harmful, or irrelevant

3. **Act:**
   - Assign improvement tasks
   - Mark deprecated items
   - Remove harmful content

## Common Quality Issues

### Issue 1: Vague Titles
```markdown
# ❌ POOR
"Best Practices"

# ✅ GOOD
"API Input Validation Best Practices"
```

### Issue 2: Missing Context
```markdown
# ❌ POOR
Pattern: "Use Redis for caching"
[No when to use, when not to use]

# ✅ GOOD
Pattern: "Use Redis for caching read-heavy API responses"
When to use: Read:write >10:1, acceptable staleness, bounded key space
When NOT to use: Real-time requirements, unbounded keys, write-heavy
```

### Issue 3: No Examples
```markdown
# ❌ POOR
"Validate inputs using schemas"

# ✅ GOOD
"Validate inputs using JSON Schema"
```python
from jsonschema import validate

schema = {
    "type": "object",
    "properties": {
        "email": {"type": "string", "format": "email"},
        "age": {"type": "integer", "minimum": 0}
    },
    "required": ["email"]
}

validate({"email": "user@example.com", "age": 25}, schema)  # ✅ Valid
validate({"email": "invalid"}, schema)  # ❌ Raises ValidationError
```
```

### Issue 4: Speculation vs. Evidence
```markdown
# ❌ POOR
"This should improve performance"

# ✅ GOOD
"Improved performance by 10x in testing (500ms → 50ms latency)"
Evidence: Applied to 3 services, avg improvement 8.3x
```

### Issue 5: Absolute Statements
```markdown
# ❌ POOR
"Always use microservices"
"Never use global variables"

# ✅ GOOD
"Use microservices when team size >8 and bounded contexts are clear"
"Avoid global variables in multi-threaded contexts (race conditions)"
```

## Summary

**Quality Principles:**
1. **Accuracy:** Knowledge reflects reality
2. **Actionability:** Provides clear guidance
3. **Completeness:** Includes all necessary context
4. **Clarity:** Easy to understand
5. **Maintainability:** Can be updated over time
6. **Evidence-Based:** Backed by data or authoritative sources

**Quality Levels:**
- **Minimum Viable:** Required for all knowledge
- **Production Quality:** Target for most knowledge
- **Exemplary:** Best-in-class, aspirational

**Quality Checklist:**
- [ ] Accurate (verified through application)
- [ ] Actionable (step-by-step guidance)
- [ ] Complete (context, examples, criteria)
- [ ] Clear (well-structured, scannable)
- [ ] Maintainable (versioned, dependencies clear)
- [ ] Evidence-based (empirical data or sources)

**Remember:**
> Quality is not perfection. Quality is fitness for purpose.
>
> Better to have one high-quality pattern that's actually used than ten low-quality ones that are ignored.

**Continuous Improvement:**
> Knowledge quality improves over time. Start at minimum viable, evolve toward production quality through application and feedback.
