# Workflow: Governance, Risk, and Decision

Support risk-aware decisions, governance thinking, and executive narrative — without compliance sign-off or invented facts.

---

## Purpose and triggers

**Purpose:** Produce decision records, risk and control considerations, and executive communications for material leadership decisions in a regulated context.

**Triggers:**
- Control gap or audit finding response planning
- Material architecture or vendor decision with regulatory impact
- AI adoption or automation decision
- Executive narrative for ExCo or Board on risk/governance topics
- Operating model change with control implications

**Do not use for:**
- Compliance or audit sign-off
- Legal advice
- Individual accountability assessment

---

## Required inputs

| Input | Description |
|---|---|
| Decision title | Clear statement of what must be decided |
| Context | Drivers, constraints, regulatory relevance (leader-supplied) |
| Evidence | Audit findings, control assessments, prior decisions |
| Options | Known options (agent may propose additional with labelling) |
| Audience | ExCo, Board, or internal leadership |
| Decision horizon | When decision is required |

---

## Lite vs full loop

| Mode | When | Phases |
|---|---|---|
| **Lite** | Internal options exploration; no external sharing | ASK → EXECUTE → REVIEW |
| **Full** | ExCo/Board; regulatory; AI adoption; material risk | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |

**Human checkpoint before EXECUTE** is mandatory in full mode.

---

## Step map

| Phase | Actions | Prompt(s) | Output |
|---|---|---|---|
| **ASK** | Clarify decision, regulatory context, evidence, audience | `prompts/ask-clarifying-questions.md` | Question log |
| **PLAN** | Structure options analysis, control considerations, narrative arc | `prompts/plan-scaffold.md` | Decision plan |
| **CRITIQUE** | Red-team risk blind spots, control gaps, stakeholder impact | `prompts/red-team-critique.md` | Risk critique |
| **CHECKPOINT** | **Leader confirms approach before EXECUTE** | — | Leader approval |
| **EXECUTE** | Draft decision record; optional HTML visual summary | — | Options paper + optional visual |
| **REVIEW** | Evidence gap analysis; governance language check | `prompts/evidence-gap-analysis.md` | Readiness verdict |
| **ITERATE** | Coaching on decision defence; revise narrative | `prompts/leadership-coaching-challenge.md` | Final pack |

---

## Evidence requirements

Minimum before EXECUTE (after leader checkpoint):

- Decision scope and out-of-scope explicitly stated
- At least two options where viable (including do-nothing/defer)
- Control considerations linked to evidence or `[Unknown]` — not effectiveness claims
- AI decisions include responsible AI section
- Recommendation framed for leader consideration only

---

## Human checkpoints

- [ ] Leader confirms decision scope and regulatory context before PLAN
- [ ] **Mandatory:** Leader approves plan and options framing before EXECUTE
- [ ] Leader records decision in template — not the agent
- [ ] Leader approves external sharing after REVIEW verdict

---

## Stop and refusal rules

**Stop and return to ASK if:**
- Leader requests compliance sign-off or "audit-ready" certification from the agent
- Insufficient evidence for external audience and leader will not accept caveats

**Refuse:**
- Invented regulatory requirements or control effectiveness claims
- Decisions made on leader's behalf
- Mapping to specific regulators (FCA, PCI, etc.) without leader-supplied context

---

## Expected outputs

| Output | Template | Required |
|---|---|---|
| Decision record / options paper | `templates/decision-record-options-paper.md` | Yes |
| Leadership visual summary (HTML) | `templates/leadership-visual-summary.html` | Optional — for ExCo one-page risk/decision summary |

Ensure HTML indicators map to evidence footnotes and companion markdown evidence table.

---

## Example opening prompt for Cursor

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/governance-risk-decision.md

Decision: {decision title}
Context: {regulatory/business drivers}
Run full loop. Pause for my approval before EXECUTE.

Evidence: {audit findings, control notes, prior decisions}
Options I'm considering: {A, B, defer}
Audience: ExCo · Decision by: {date}
```
