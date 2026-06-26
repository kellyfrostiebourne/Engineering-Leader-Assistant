# Workflow: Technical Research Brief

Produce evidence-led technical research to support platform, architecture, and tooling decisions — without implementation work.

---

## Purpose and triggers

**Purpose:** Structure technical investigation into a decision-ready brief with sourced findings, options, and trade-offs.

**Triggers:**
- Evaluating platforms, vendors, or architecture patterns
- AI tooling or automation adoption research
- Build vs buy analysis
- Technical due diligence ahead of a material decision

**Do not use for:**
- Writing or reviewing production code
- Implementation planning or sprint breakdown
- Vendor selection sign-off (leader decides)

---

## Required inputs

| Input | Description |
|---|---|
| Research question | Specific, decision-oriented question |
| Constraints | Budget, regulatory, timeline, integration requirements |
| Evidence | Existing evaluations, POC results, documentation, leader notes |
| Decision horizon | When the decision is needed |
| Audience | Who will consume the brief (default: leader + ExCo if material) |

---

## Lite vs full loop

| Mode | When | Phases |
|---|---|---|
| **Lite** | Exploratory scan; no imminent decision | ASK → EXECUTE → REVIEW |
| **Full** | Material spend, architecture change, or AI adoption | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |

This workflow is **ASK/PLAN heavy** and **CRITIQUE before EXECUTE** on material topics.

---

## Step map

| Phase | Actions | Prompt(s) | Output |
|---|---|---|---|
| **ASK** | Sharpen research question; identify sources and gaps | `prompts/ask-clarifying-questions.md` | Question log |
| **PLAN** | Define research scope, source plan, evaluation criteria | `prompts/plan-scaffold.md` | Research plan |
| **CRITIQUE** | Challenge source quality, bias, gaps before drafting | `prompts/red-team-critique.md` | Source and assumption critique |
| **EXECUTE** | Draft decision record with research findings emphasis | — | Decision record / options paper |
| **REVIEW** | Verify every finding is sourced or labelled | `prompts/evidence-gap-analysis.md` | Traceability matrix |
| **ITERATE** | Address gaps; optional coaching on decision framing | `prompts/leadership-coaching-challenge.md` | Revised brief |

---

## Evidence requirements

Minimum before EXECUTE:

- Research question stated as a decision-oriented question
- Sources classified: primary documentation, leader input, third-party analysis, [Unknown]
- No vendor marketing claims presented as [Evidence] without independent corroboration
- AI capability claims include limitations and responsible AI section if applicable

---

## Human checkpoints

- [ ] Leader confirms research question and evaluation criteria before PLAN
- [ ] Leader reviews source quality assessment in CRITIQUE
- [ ] Leader selects preferred option — agent does not decide
- [ ] For AI tooling: leader explicitly accepts residual risk section

---

## Stop and refusal rules

**Stop and return to ASK if:**
- Research question is too vague to produce a decision-ready brief
- Required sources are unavailable and leader will not accept [Unknown] labels

**Refuse:**
- Presenting vendor collateral as independent evidence without labelling
- Implementation code or architecture diagrams intended for production use (Mermaid for explanation is acceptable)

---

## Expected outputs

| Output | Template | Required |
|---|---|---|
| Decision record / options paper | `templates/decision-record-options-paper.md` | Yes |

Use the **Research findings** and **Responsible AI considerations** sections prominently.

Optional: feed output into `strategy-synthesis` workflow as input evidence.

---

## Example opening prompt for Cursor

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/technical-research-brief.md

Research question: Should {Organisation} adopt {tool/platform} for {use case}?
Run full loop. Decision needed by {date}.

Sources I have: {list documents, POC notes, URLs}
Constraints: {regulatory, budget, integration}
Evaluation criteria: {criteria}
```
