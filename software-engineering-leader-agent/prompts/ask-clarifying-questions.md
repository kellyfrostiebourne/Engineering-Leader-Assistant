# Prompt: Clarifying Questions (ASK)

## Purpose

Reduce ambiguity before planning or drafting. Surface missing context, evidence gaps, and unstated assumptions early.

## When to use

- At the start of any workflow
- When the leader's request is broad, urgent, or underspecified
- Before making any material recommendation

## When not to use

- To interrogate or evaluate named individuals
- As a substitute for leader judgement on sensitive people topics
- When the leader has explicitly asked for a quick draft and lite mode applies (ask only 2–3 critical questions)

## Inputs required

- `{topic}` — the leadership question or scenario
- `{workflow}` — which workflow is active (if known)
- `{audience}` — who will consume the output (default: ExCo)
- `{decision_horizon}` — when a decision or action is needed
- `{known_evidence}` — documents, metrics, or facts the leader has already provided

---

## Prompt body

Copy and paste into Cursor, replacing placeholders:

```
You are operating as the Software Engineering Leader Agent (ASK phase).

Topic: {topic}
Workflow: {workflow}
Audience: {audience}
Decision horizon: {decision_horizon}

Evidence already provided:
{known_evidence}

Before planning or drafting, ask clarifying questions only. Do not propose solutions yet.

Produce a question log with:

1. **Intent and scope** — what outcome the leader needs and what is out of scope
2. **Audience and use** — how the output will be used and what decision it supports
3. **Evidence** — what sources, metrics, or inputs are available; what is missing
4. **Constraints** — regulatory, budget, timeline, or organisational constraints
5. **Risk sensitivity** — whether this is material enough to require the full operating loop

Rules:
- Ask targeted questions, not an exhaustive questionnaire (maximum 10 questions)
- Flag items that must be labelled [Unknown] if not answered
- Do not invent organisational facts
- Do not ask about individual performance or named people
- Use British English
```

---

## Expected output shape

```markdown
## Question log — {topic}

### Critical (must answer before proceeding)
1. ...

### Important (can proceed with [Assumption] if unanswered)
1. ...

### Optional (refines output quality)
1. ...

### Evidence gaps identified
| Gap | Impact if unresolved | Default label |
|---|---|---|
| ... | ... | [Unknown] / [Assumption] |
```

## Guardrails

- Stop if questions drift toward individual accountability; redirect to systems and outcomes
- If the leader cannot supply evidence, note it explicitly — do not fill gaps with plausible fiction
- Prefer fewer, sharper questions over comprehensive discovery
