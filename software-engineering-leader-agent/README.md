# Software Engineering Leader Agent

A lightweight, markdown-first agent system for senior software engineering leaders in regulated financial technology organisations. Supports leadership thinking, documentation, and visual communication — not coding.

---

## What this is

- A reusable agent instruction set for **Cursor**
- Five leadership **workflows** (strategy, research, health, delivery, governance)
- Five reusable **prompts** for the operating loop
- Four **markdown templates** and two **HTML visual shells**
- Grounded in **ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE**

## What this is not

- A coding assistant or architecture implementation tool
- An HR, performance management, or people analytics system
- A source of organisational facts — it structures *your* evidence
- A decision-maker — **you** decide; the agent advises and drafts
- An autonomous agent — it does not act outside your Cursor session

---

## Quick start in Cursor

### 1. Reference the agent

In a new chat, @-mention the primary agent file:

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md
```

### 2. Choose a workflow

| If you need… | Use |
|---|---|
| Strategy or operating model docs | `workflows/strategy-synthesis.md` |
| Technical platform or tooling research | `workflows/technical-research-brief.md` |
| Engineering health review | `workflows/engineering-health-review.md` |
| Release or programme readiness | `workflows/delivery-assurance.md` |
| Risk, governance, or decision memo | `workflows/governance-risk-decision.md` |

### 3. Provide context and run the loop

Example:

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/strategy-synthesis.md

I need an ExCo briefing on our platform modernisation strategy.
Decision needed by 15 July. Run the full operating loop.

Context: {your context}
Evidence: {documents, metrics, notes}
```

### 4. Use prompts at each phase

| Phase | Prompt file |
|---|---|
| ASK | `prompts/ask-clarifying-questions.md` |
| PLAN | `prompts/plan-scaffold.md` |
| CRITIQUE | `prompts/red-team-critique.md` |
| REVIEW | `prompts/evidence-gap-analysis.md` |
| ITERATE (coaching) | `prompts/leadership-coaching-challenge.md` |

Copy the prompt body from each file, replace `{placeholders}`, and paste into Cursor.

### 5. Produce outputs from templates

During EXECUTE, ask the agent to populate the workflow's template from `templates/`.

---

## Operating loop walkthrough

**Example session:** Delivery assurance for a regulatory release (full loop)

1. **ASK** — Agent asks about scope, target date, readiness criteria, and available evidence. You answer or accept `[Assumption]` labels.
2. **PLAN** — Agent outlines readiness dimensions, evidence plan, and template structure. You confirm.
3. **CRITIQUE** — Agent pre-mortems failure modes and challenges overconfidence. You review issues.
4. **EXECUTE** — Agent drafts `delivery-assurance-summary.md` with evidence table populated from your inputs.
5. **REVIEW** — Agent runs evidence gap analysis. Verdict: Ready / Ready with caveats / Not ready.
6. **ITERATE** — You request revisions or invoke coaching challenge before sharing.

**Lite mode example:** Internal engineering health sync

1. **ASK** — 2–3 critical questions only
2. **EXECUTE** — Draft memo with evidence labels
3. **REVIEW** — Quick gap check

Skip PLAN, CRITIQUE, and ITERATE unless the topic becomes material.

---

## Choosing markdown vs HTML outputs

| Format | Use when |
|---|---|
| **Markdown templates** | Default for all leadership documents; audit trail; editing in Cursor |
| **Mermaid in markdown** | Process flows and operating models during working sessions |
| **HTML templates** | Executive one-pagers where visual layout aids comprehension |

HTML templates (self-contained, inline CSS only):

- `templates/strategy-one-pager.html` — strategy and ExCo briefings
- `templates/leadership-visual-summary.html` — governance, risk, delivery, or health summaries

**Rule:** Every HTML claim must map to the companion markdown evidence table. Do not share HTML without completing REVIEW.

---

## Evidence and audit trail practices

### Evidence labels

| Label | Meaning |
|---|---|
| `[Evidence]` | Supported by a source you provided or that can be cited |
| `[Inference]` | Reasoned from evidence — basis stated |
| `[Assumption]` | Working assumption — needs confirmation |
| `[Unknown]` | Required information not yet available |
| `[REDACTED]` | Confidential content not reproduced |

### What to save

- Final markdown outputs with evidence tables completed
- Your answers to ASK phase questions
- REVIEW verdict and any caveats accepted
- Leader decision checkboxes signed off by you

### What to redact

Mark sensitive content `[REDACTED]` in outputs. Do not paste credentials, customer data, or unreleased financial figures into Cursor unless your organisation's AI policy permits it.

### Defaults

- **Audience:** ExCo
- **Organisation placeholder:** `{Organisation}`
- **Confidentiality:** `[REDACTED]`

---

## Lite mode vs full loop

| | Lite | Full |
|---|---|---|
| **Phases** | ASK → EXECUTE → REVIEW | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |
| **When** | Low-stakes, internal, time-boxed | Material risk, external audience, AI adoption, regulatory |
| **Coaching** | Optional | Recommended before high-stakes sharing |

Each workflow file specifies lite vs full guidance.

---

## Folder structure

```
software-engineering-leader-agent/
├── SOFTWARE_ENGINEERING_LEADER_AGENT.md
├── README.md
├── POST_IMPLEMENTATION_REVIEW.md
├── workflows/          (5 workflows)
├── prompts/            (5 prompts)
└── templates/          (4 markdown + 2 HTML)
```

---

## v1 limitations and when to stop

**Stop using the agent and seek human specialist input when:**

- You need legal, compliance, or audit sign-off
- The topic requires HR or people management judgement
- Evidence is too thin for the intended audience and gaps cannot be acceptably labelled
- The agent is asked to invent facts, metrics, or individual assessments

**v1 does not include:**

- Jira, Confluence, or metrics tool integration
- Org-specific regulatory control catalogues
- Maturity models, OKR/RACI libraries, or scoring systems
- Automated or scheduled agent runs

---

## Maintenance

### Adding a workflow without bloating v1

Before adding a new file, ask:

1. Does it map to one leadership scenario and one output type?
2. Can an existing workflow be extended instead?
3. Will it duplicate governance boilerplate already in templates?

If yes to duplication, defer to v2 or extend an existing workflow's "triggers" section.

### Versioning

Manual changelog — update the line below when you change the system:

**Changelog:** v1.0 — June 2026 — Initial release (18 files).

---

## Coaching mode

Invoke `prompts/leadership-coaching-challenge.md` when you want constructive challenge on your reasoning — not validation, not performance review.

```
@prompts/leadership-coaching-challenge.md

Coach me on this decision before I take it to ExCo.
Topic: {topic}
My current leaning: {option}
Stakes: {what happens if wrong}
```

---

## Related files

- Agent instructions: [SOFTWARE_ENGINEERING_LEADER_AGENT.md](SOFTWARE_ENGINEERING_LEADER_AGENT.md)
- Post-implementation review: [POST_IMPLEMENTATION_REVIEW.md](POST_IMPLEMENTATION_REVIEW.md)
