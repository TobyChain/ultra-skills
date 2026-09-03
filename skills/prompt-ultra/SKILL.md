---
name: prompt-ultra
description: Inspect, strengthen, and clarify user inputs before executing consequential or medium-to-high-complexity work. Invoke implicitly for any task involving multiple dependent steps, autonomous decisions, ambiguous requirements, material file or system changes, architecture or strategy choices, research synthesis, external side effects, significant cost or risk, or acceptance criteria that are not already explicit; also invoke when the user asks for prompt review, prompt optimization, requirements clarification, task scoping, a task brief, or "prompt-ultra". Build a complete execution contract covering objective, context, deliverable, scope, constraints, evidence, acceptance tests, authority, and exception handling, and resolve material gaps before execution. Skip for simple, low-risk, easily reversible requests whose intent and success condition are obvious.
---

# Prompt Ultra

Turn an underspecified complex request into an executable task contract before substantial action. Treat clarity as a gate: inspect first, resolve only material gaps, confirm the resulting contract, then execute.

## Core rules

1. Preserve the user's intent. Strengthen the request without silently expanding its objective, authority, or scope.
2. Inspect available conversation context and accessible sources before asking the user for information that can be discovered safely.
3. Ask only questions whose answers could materially change the approach, permissions, risk, deliverable, or acceptance result.
4. Ask questions in small, prioritized batches. Explain the decision each question controls when that is not obvious.
5. Record safe defaults as explicit assumptions. Do not block execution on preferences with low consequence.
6. Do not begin substantive implementation while a critical gap remains. Read-only discovery needed to clarify the task is allowed.
7. Stop asking when the execution contract is sufficient. Do not seek impossible certainty or repeat an answered question.
8. If the user explicitly requests immediate action, use safe assumptions for non-critical gaps and proceed; stop only for missing authority, irreversible risk, or choices that would materially change the result.

## 1. Decide whether to gate the task

Classify the request before acting.

### Gate required

Use the full inspection when any condition applies:

- The work has multiple dependent stages or competing valid approaches.
- The agent must make product, architecture, research, policy, or business decisions.
- The task changes important files, data, infrastructure, accounts, or external systems.
- Failure could cause meaningful loss, disclosure, cost, downtime, compliance risk, or hard-to-reverse effects.
- The requested deliverable, audience, scope, evidence standard, or acceptance criteria are unclear.
- The user delegates broad autonomy such as "handle it", "finish it", or "do whatever is needed".
- The work depends on current facts, third-party coordination, credentials, approvals, or unavailable inputs.
- The user explicitly requests prompt strengthening or requirements clarification.

### Lightweight gate

For medium-complexity work with a clear objective, inspect all fields internally, state any consequential assumptions briefly, and ask only about critical gaps. Continue in the same turn when none remain.

### Skip

Skip the gate for simple explanations, translations, formatting changes, obvious one-file edits, or other low-risk and reversible tasks with a clear output and success condition. Honor an explicit invocation even for a simple task.

## 2. Build the input map

Extract and normalize these fields from the request and prior context. Do not ask for fields that do not apply.

| Field | Question to resolve |
| --- | --- |
| Objective | What outcome must exist when the task is done? |
| Motivation | Why is the outcome needed, and which decision or workflow will it support? |
| Audience | Who will use or evaluate the result, and what do they already know? |
| Inputs | Which files, data, links, systems, and source-of-truth materials apply? |
| Deliverable | What artifact, action, format, location, language, and level of detail are required? |
| Scope | What is included, excluded, and explicitly out of bounds? |
| Constraints | Which technical, legal, policy, style, time, cost, compatibility, and dependency limits apply? |
| Evidence | May external sources be used? Must claims be cited, current, reproducible, or based only on supplied material? |
| Authority | May the agent edit, run tests, install dependencies, access networks, contact people, deploy, publish, or delete? |
| Acceptance | What observable checks determine success? |
| Priorities | How should quality, speed, cost, safety, compatibility, and completeness be traded off? |
| Exceptions | What should happen on missing data, conflicting instructions, failed validation, partial access, or newly discovered risk? |

Label each applicable field as:

- **Known**: explicitly supplied or reliably discovered.
- **Assumed**: a safe, reversible default that does not materially redirect the task.
- **Critical gap**: missing information that could change the result, authority, or risk.
- **Deferred**: intentionally decided later under an agreed rule.

## 3. Resolve ambiguity

Before questioning, inspect conversation history, repository instructions, nearby files, provided links, and other read-only evidence within scope. Never use discovery to perform the substantive task early.

Ask the user when a gap controls one of these decisions:

- selecting between materially different outputs or implementations;
- crossing an authorization boundary or causing an external side effect;
- accepting destructive, security, privacy, financial, legal, or production risk;
- defining which source is authoritative when sources conflict;
- determining a success criterion that cannot be inferred from context.

When asking:

1. Lead with the most consequential unresolved decision.
2. Prefer one to three short questions per round.
3. Offer a recommended default when alternatives are known.
4. Make options mutually distinct and state their practical tradeoff.
5. Incorporate each answer into the input map before asking another round.

Do not ask the user to choose implementation details the agent can responsibly determine. Do not ask for confirmation merely to protect the agent from making a normal, reversible decision.

## 4. Produce the execution contract

Once no critical gaps remain, present a compact contract proportional to the task:

```markdown
Task: <one-sentence outcome>

Context and audience:
- ...

Deliverable:
- ...

In scope:
- ...

Out of scope:
- ...

Constraints and authority:
- ...

Evidence and verification:
- ...

Acceptance criteria:
- ...

Assumptions:
- ...

Exception handling:
- If <condition>, then <action/report/escalation>.
```

For high-risk or highly ambiguous work, ask the user to confirm the contract before execution. For medium-complexity work, show the compact contract or summarize the consequential assumptions and proceed in the same turn. Do not require ritual confirmation when the user has already supplied all material decisions.

## 5. Strengthen the operative prompt

Convert the contract into direct instructions for execution. Include only applicable clauses:

```text
Complete <specific outcome> for <audience/use case>.

Use <authoritative inputs>. Produce <deliverable and format> at <location, if any>.
Include <scope>. Exclude <out-of-scope work>.
Follow <constraints and priorities>. You are authorized to <allowed actions>; do not <forbidden actions>.
When information is missing but non-critical, use <default> and disclose the assumption.
When <critical exception> occurs, <stop, retry, choose fallback, or ask>.
Verify completion with <tests/checks>. The task is complete only when <acceptance criteria>.

Use direct, literal, precise language. Remove repetition, stock phrases, unnecessary setup, ornamental metaphor, and other mannered prose. Keep all decision-relevant facts, caveats, and evidence. Distinguish facts, inferences, assumptions, and recommendations.
```

Prefer observable instructions over adjectives. Replace "make it professional" with the audience, tone, structure, evidence, and use case. Replace "be concise" with what to preserve, what to remove, and an appropriate length or hierarchy.

## 6. Execute and control drift

After the gate passes:

1. Execute against the contract rather than the user's raw wording alone.
2. Keep changes and tests within scope. Report useful out-of-scope findings without acting on them.
3. Treat discovered facts that invalidate an assumption as a new critical gap. Pause only if they materially alter risk, authority, or the agreed result.
4. Complete the full authorized task, including relevant verification. Do not stop at advice when implementation was requested.
5. At handoff, report the delivered outcome, verification performed, assumptions used, and unresolved exceptions.

## Quality check

Before execution, confirm:

- The final outcome and deliverable are unambiguous.
- Background and audience are sufficient for consequential decisions.
- Inputs and sources of truth are identified.
- Included and excluded scope are explicit.
- Constraints, permissions, and external side effects are understood.
- Evidence requirements and freshness expectations are defined.
- Acceptance criteria are observable.
- Missing-data, conflict, failure, and fallback behavior are defined where relevant.
- Every remaining assumption is safe, reversible, and disclosed.
- No unresolved critical gap remains.

Before handoff, confirm:

- Every requested output exists.
- Verification matches the task's risk.
- Facts, inferences, assumptions, and recommendations are distinguishable.
- The response is complete but compact and contains no mannered prose.
