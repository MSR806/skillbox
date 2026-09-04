---
name: planning
description: Use whenever planning implementation, research, debugging, migrations, rollouts, refactors, or delegated work; creates plans by establishing reality, ownership, and the immediate objective so each next action follows naturally.
---

# Planning

Create plans that make the correct next action obvious from the current state. Do not try to make a plan reliable by surrounding it with prohibitions. Give each participant enough reality, responsibility, and direction to act well.

## Core Rule

Before prescribing an action, establish:

- what has already happened
- what is known and settled
- what remains unresolved
- who owns the next decision or action
- what outcome the next action must produce

An unsuitable action is often reasonable from an incomplete or misleading working state. Repair the state before adding more rules.

## Build the Plan

### 1. Establish Reality

Inspect the relevant code, documents, systems, and prior decisions before planning. Separate confirmed facts from assumptions and open questions.

State where the work currently stands. Do not make the reader reconstruct it from scattered evidence.

### 2. Define the Outcome

Describe the observable end state, not a broad aspiration. The outcome should make it possible to tell whether the plan succeeded.

Prefer:

> Requests use the new authentication path, existing sessions remain valid, and integration tests cover both behaviors.

Over:

> Improve authentication.

### 3. Define Ownership

Make clear which decisions belong in this plan and which are already settled or handled elsewhere.

- Do not reopen upstream decisions without new evidence.
- Do not assign one step responsibility for the entire pipeline.
- Identify handoffs when another person, agent, service, or later phase owns the next result.
- Ask a question only when the answer materially changes the plan.

### 4. Choose the Immediate Objective

Give every step one concrete job. A step should say what must change or be learned and why that result enables the following work.

Prefer:

> Trace token creation and validation to identify every call site that must move together.

Over:

> Investigate authentication.

### 5. Order by State Transitions

Sequence work so each step creates the state required by the next. Use dependencies, uncertainty, and risk to determine order rather than listing files or activities arbitrarily.

For each step, make clear:

- the starting state it relies on
- the action to take
- the result it must leave behind
- how that result will be verified

Combine details when the transition is obvious. Do not force every step into a rigid template.

### 6. Use Positive Direction

Describe the useful action and its destination. Use prohibitions only for genuine boundaries such as safety, compatibility, scope, or irreversible operations.

Prefer:

> Continue from the existing parser and add the new field at its input boundary.

Over:

> Do not rewrite the parser. Do not add another parser. Do not change unrelated fields.

### 7. Make Verification Part of the Plan

Attach verification to the behavior or decision it proves. Include relevant tests, static checks, builds, runtime checks, or review points. Avoid a generic final step that says only "test everything."

## Delegating Work

When a plan delegates a step to another agent or person, shape the task with four elements:

1. **Starting state:** Where the work is and what has already happened.
2. **Available context:** Facts, artifacts, constraints, and decisions they may rely on.
3. **Ownership:** The decision or result they are responsible for, including relevant boundaries.
4. **Immediate objective:** What their next response or action must accomplish.

Example:

> The request path has already been traced to `AuthMiddleware`, and token format changes are out of scope. Use the existing middleware and test fixtures as established context. You own identifying the smallest validation change and its regression coverage. Return the affected code paths, recommended edit, and evidence that existing sessions remain valid.

## Writing the Plan

Use the smallest structure that keeps the work unambiguous:

1. **Current state:** Confirmed reality, relevant prior decisions, and unresolved facts.
2. **Target state:** The observable outcome and success criteria.
3. **Plan:** Ordered state transitions, each with a concrete purpose and verification.
4. **Open decisions:** Only questions that block or materially alter execution, with their owner.

Omit a section when it adds no useful information. Scale detail to uncertainty and risk: a local change may need three concise steps; a migration may need phases, rollback conditions, and handoffs.

## Diagnose Weak Plans

Before adding another instruction or step, ask:

- What does the actor know when this step begins?
- What do they believe their main job is?
- What has already been established?
- What belongs to this step, and what belongs elsewhere?
- Why would an unsuitable action seem reasonable from this state?
- What missing purpose, fact, or ownership boundary would make the right action natural?
- What is the smallest change that removes that ambiguity?

Common symptoms:

- **Repeated discovery:** The plan did not treat prior findings as established state.
- **Steps that overlap:** Ownership is unclear.
- **Generic verbs:** The immediate objective or intended result is missing.
- **Long lists of warnings:** Positive direction is missing.
- **Premature implementation detail:** Reality has not been inspected or a decision has not been made.
- **A detached testing phase:** Verification was not tied to each state transition.

## Final Check

- Is the starting reality explicit and evidence-based?
- Is the target state observable?
- Does each step have one immediate objective?
- Does each step create what the next step needs?
- Are ownership and handoffs clear?
- Are settled decisions treated as settled?
- Do constraints protect real boundaries rather than substitute for direction?
- Is verification specific to the outcome?
- Could any step be removed without losing necessary state?

The plan is ready when a capable reader can take the next action without reconstructing context, reopening settled decisions, or guessing what success means.
