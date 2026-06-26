# Workflow: Strategy Synthesis

Turn fragmented inputs into coherent strategy and tactical documentation for executive and engineering leadership audiences.

---

## Purpose and triggers

**Purpose:** Synthesise strategic intent, operating model shifts, and tactical plans into clear, evidence-led documentation.

**Triggers:**
- Quarterly or annual engineering strategy refresh
- New initiative or programme shaping
- Operating model change (autonomy, guardrails, platform model)
- Preparing ExCo or Board alignment on engineering direction

**Do not use for:**
- Individual accountability tracking
- Sprint-level planning or task breakdown
- Code or architecture implementation

---

## Required inputs

| Input | Description |
|---|---|
| Context | Current situation, drivers for change, constraints |
| Evidence | Metrics, prior decisions, leader-provided documents |
| Decision horizon | When alignment or funding decisions are needed |
| Audience | Default: ExCo; may be Board or engineering peers |
| Optional | Output from `technical-research-brief` workflow if research informs strategy |

---

## Lite vs full loop

| Mode | When | Phases |
|---|---|---|
| **Lite** | Internal alignment memo; low material risk | ASK → EXECUTE → REVIEW |
| **Full** | ExCo/Board audience; material spend or regulatory impact | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |

---

## Step map

| Phase | Actions | Prompt(s) | Output |
|---|---|---|---|
| **ASK** | Clarify strategic question, audience, horizon, available evidence | `prompts/ask-clarifying-questions.md` | Question log |
| **PLAN** | Define scope, themes, evidence plan, output structure | `prompts/plan-scaffold.md` | Plan outline |
| **CRITIQUE** | Red-team assumptions, stakeholder blind spots, strategic risks | `prompts/red-team-critique.md` | Issue list |
| **EXECUTE** | Draft using templates; add Mermaid diagrams where helpful | — | Executive briefing + optional HTML one-pager |
| **REVIEW** | Evidence gap analysis; verify ExCo readiness | `prompts/evidence-gap-analysis.md` | Review checklist |
| **ITERATE** | Revise based on feedback; optional coaching challenge | `prompts/leadership-coaching-challenge.md` | Revised draft |

---

## Evidence requirements

Minimum before EXECUTE:

- At least one `[Evidence]` source for the strategic problem statement
- Explicit `[Assumption]` or `[Unknown]` labels for market, regulatory, or capacity claims
- No unsupported growth, savings, or timeline claims

---

## Human checkpoints

- [ ] Leader confirms strategic question and audience before PLAN
- [ ] Leader reviews CRITIQUE issues before EXECUTE
- [ ] Leader approves recommendation framing before external sharing
- [ ] Leader signs decision checkboxes in template before ExCo presentation

---

## Stop and refusal rules

**Stop and return to ASK if:**
- Evidence is insufficient for the stated audience
- Request shifts to individual performance evaluation

**Refuse:**
- Invented market data, competitor facts, or organisational policies
- Strategy documents that assign blame to named individuals

---

## Expected outputs

| Output | Template | Required |
|---|---|---|
| Executive briefing note | `templates/executive-briefing-note.md` | Yes |
| Strategy one-pager (HTML) | `templates/strategy-one-pager.html` | Optional — when visual layout aids ExCo communication |

Ensure HTML one-pager evidence footnotes match the markdown evidence table.

---

## Example opening prompt for Cursor

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/strategy-synthesis.md

I need to synthesise our engineering platform strategy for ExCo ahead of Q3 planning.
Run the strategy synthesis workflow in full loop mode.

Context: {paste context}
Evidence: {paste or reference documents}
Decision needed by: {date}
```
