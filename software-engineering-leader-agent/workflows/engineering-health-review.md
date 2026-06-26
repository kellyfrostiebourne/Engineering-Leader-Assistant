# Workflow: Engineering Health Review

Conduct a systems-level engineering health review — focusing on delivery, quality, platform, and operating model themes without individual assessment.

---

## Purpose and triggers

**Purpose:** Produce an evidence-led health review memo suitable for leadership awareness, improvement planning, or pre-audit preparation.

**Triggers:**
- Monthly or quarterly engineering health review
- Pre-audit or regulatory inspection preparation
- Post-incident systemic review (not blame-oriented)
- Operating model effectiveness check

**Do not use for:**
- Named individual or team scoring
- Performance management input
- League tables or comparative people rankings

---

## Required inputs

| Input | Description |
|---|---|
| Review period | Time window under review |
| Scope | Systems, platforms, programmes in scope |
| Evidence | Metrics dashboards, incident reports, delivery data (leader-provided) |
| Audience | Default: ExCo; may be internal leadership only |
| Prior reviews | Previous health memos for trend context (optional) |

---

## Lite vs full loop

| Mode | When | Phases |
|---|---|---|
| **Lite** | Internal leadership sync; no external sharing | ASK → EXECUTE → REVIEW |
| **Full** | ExCo or audit-preparation audience | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |

---

## Step map

| Phase | Actions | Prompt(s) | Output |
|---|---|---|---|
| **ASK** | Confirm scope, metrics available, audience, review period | `prompts/ask-clarifying-questions.md` | Question log |
| **PLAN** | Define health themes, evidence plan, finding structure | `prompts/plan-scaffold.md` | Review plan |
| **CRITIQUE** | Challenge metric interpretation, missing data, false precision | `prompts/red-team-critique.md` | Critique of planned assessment |
| **EXECUTE** | Draft health review memo with evidence table | — | Engineering health review memo |
| **REVIEW** | Evidence-led traceability check | `prompts/evidence-gap-analysis.md` | Readiness verdict |
| **ITERATE** | Revise; optional coaching on messaging | `prompts/leadership-coaching-challenge.md` | Revised memo |

---

## Evidence requirements

Minimum before EXECUTE:

- Metrics referenced must include definition, source, and measurement date — or be labelled `[Unknown]`
- **No numeric health scores or RAG ratings** without the above
- Incidents and trends referenced by theme, not by individual accountability
- Security and compliance observations framed as considerations, not sign-off

---

## Human checkpoints

- [ ] Leader confirms metrics and sources are accurate
- [ ] Leader reviews CRITIQUE on false precision before EXECUTE
- [ ] Leader approves recommendations before ExCo sharing
- [ ] Leader completes decision checkboxes in template

---

## Stop and refusal rules

**Stop and return to ASK if:**
- Request includes individual performance evaluation
- Leader asks for team league tables or comparative people rankings

**Refuse:**
- Unsupported RAG dashboards or invented metrics
- Blame-oriented incident narratives naming individuals

---

## Expected outputs

| Output | Template | Required |
|---|---|---|
| Engineering health review memo | `templates/engineering-health-review-memo.md` | Yes |

Optional Mermaid diagram within memo for health area relationships.

---

## Example opening prompt for Cursor

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/engineering-health-review.md

Run a Q2 engineering health review for ExCo.
Review period: {dates}
Scope: {platforms/programmes}
Evidence: {paste metrics, incident summaries, delivery data}

Do not assess individuals. Label all gaps as [Unknown].
```
