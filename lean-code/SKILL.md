---
name: lean-code
description: Use when writing, reviewing, or refactoring code, especially first versions or requests for simple, minimal, direct, or less code; favors the smallest correct implementation over speculative abstractions and defenses.
---

# Lean Code

Build the smallest direct implementation that satisfies current requirements and concrete failure modes.

## Core Rule

Validate data when it enters from an untrusted boundary, trust values created by the application, and validate externally required output once.

Do not design for hypothetical consumers, compatibility, concurrency, scale, or operational needs.

## Current Reality First

- Implement requirements that exist now.
- Preserve shipped behavior, persisted data, and external contracts when they actually exist.
- Do not add compatibility before anything has shipped.
- Do not add migrations before data needs migrating.
- Do not add concurrency handling before concurrent writers exist.
- Do not make values configurable without an operational need.
- Do not support speculative formats, providers, volumes, or edge cases.
- Let natural exceptions fail when they already explain the problem.

## Trust Boundaries

Validation belongs where the application does not control the value:

- user or deployment input
- external service responses
- imported files or messages
- generated responses from probabilistic systems
- externally required output contracts

Internal transport objects should normally be plain data.

```python
# Avoid: trusted internal data validates itself repeatedly.
@dataclass
class InternalResult:
    location: str
    count: int

    def __post_init__(self):
        if not self.location:
            raise ValueError("missing location")
        if self.count < 0:
            raise ValueError("invalid count")

# Prefer: validate before constructing it, then use plain data.
@dataclass
class InternalResult:
    location: str
    count: int
```

## Direct Code

- Prefer direct control flow over factories, adapters, managers, and wrappers.
- Do not create a helper for a one-line conversion.
- Abstract only after real repetition exists.
- Prefer language and standard-library capabilities over custom utilities.
- Prefer supported SDK behavior over custom transport, parsing, or retry frameworks.
- Keep cohesive behavior together when splitting it would hide the flow.
- Keep internal helpers private until a real consumer needs an API.

```python
# Avoid
def optional_number(value):
    try:
        return int(value) if value is not None else None
    except (TypeError, ValueError):
        return None

result = optional_number(trusted_value)

# Prefer when the contract already guarantees number-or-null.
result = trusted_value
```

```python
# Avoid rebuilding retry behavior supplied by a dependency.
result = custom_retry(lambda: client.request())

# Prefer the supported client option.
client = SDKClient(retries=2)
result = client.request()
```

## Return What Is Needed

Do not wrap required data in unused accounting or metadata structures.

```python
# Avoid
return ProcessingReport(
    items=processed,
    seen=seen,
    skipped=skipped,
    accepted=len(processed),
)

# Prefer
return processed
```

Keep a count, metric, or metadata field only when a person or automated decision consumes it.

## Data Representations

- Use simple native data structures internally.
- Keep framework and serialization types at their boundaries.
- Do not turn internal handoffs into public contracts.
- Prefer inspectable internal state when persistence is needed.
- Model only domain values that exist in the current system.
- Avoid multiple representations of the same information without a concrete need.

## Resilience

Keep resilience for demonstrated risks:

- privacy and access control
- authentication and secure transport
- external network failures
- server-directed throttling
- atomic persistence where partial output is dangerous
- recovery for expensive interruptible work
- required final output validation

Do not remove explicitly required behavior merely because it is complex. Explicit requirements override the simplicity default.

## Ask Before Adding Complexity

Ask before adding:

- observability or notification systems
- compatibility layers
- metadata contracts
- factories or dependency injection
- generic wrappers
- dependencies or infrastructure
- speculative validation
- custom retry frameworks
- reconciliation or concurrency machinery

State the concrete problem, the smallest option, and the tradeoff before proceeding.

## Implementation Workflow

1. Identify the exact required behavior.
2. Identify real trust boundaries.
3. List concrete failure modes.
4. Implement the direct happy path.
5. Add only boundary handling required by those failures.
6. Search for helpers, validators, metrics, wrappers, and state objects that can be deleted.
7. Test observable behavior rather than internal structure.
8. Run the relevant tests, static checks, build, and runtime smoke check.

## Review Questions

For every piece of complexity, ask:

- Who consumes this?
- What exact failure does it prevent?
- Is the value external or internally generated?
- Does the compatibility or concurrency need exist today?
- Can the language, standard library, or dependency handle it?
- Would the natural exception already be sufficient?
- Is the test protecting behavior or preserving machinery?

If there is no concrete answer, delete or avoid the complexity.

## Testing

Prefer tests for observable behavior, trust boundaries, required recovery, and external contracts.

Avoid tests that freeze internal object layouts, helper existence, repeated validation layers, or speculative compatibility behavior.

## Final Check

- Is this the shortest correct path?
- Did I add a concept the product does not currently need?
- Is validation limited to real trust boundaries?
- Did I use existing language, library, or SDK behavior first?
- Are configuration, metrics, and metadata consumed?
- Do tests protect behavior rather than implementation?
- Did I preserve every explicit requirement?

If the answers expose speculative machinery, remove it.
