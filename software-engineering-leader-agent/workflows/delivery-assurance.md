# Workflow: Delivery Assurance

Assess programme or release readiness and delivery confidence — without blame or individual performance assessment.

---

## Purpose and triggers

**Purpose:** Produce a delivery assurance summary supporting go/no-go or conditional go decisions with evidence-led readiness assessment.

**Triggers:**
- Major release or cutover planning
- Regulatory deadline or audit-related delivery milestone
- Programme stage-gate review
- Post-delay recovery assurance

**Do not use for:**
- Assigning blame to named individuals
- Sprint retrospectives focused on people
- Individual performance documentation

---

## Required inputs

| Input | Description |
|---|---|
| Programme / release | Name, scope, target date |
| Deliverables | What must be complete for readiness |
| Evidence | Test results, defect status, dependency trackers, runbook status (leader-provided) |
| Audience | Who needs the go/no-go decision |
| Contingency | Rollback and deferral options known to the leader |

---

## Lite vs full loop

| Mode | When | Phases |
|---|---|---|
| **Lite** | Low-risk internal release; leader already has strong evidence | ASK → EXECUTE → REVIEW |
| **Full** | Regulatory, customer-facing, or material business impact | ASK → PLAN → CRITIQUE → EXECUTE → REVIEW → ITERATE |

**CRITIQUE and REVIEW are emphasised** in full mode.

---

## Step map

| Phase | Actions | Prompt(s) | Output |
|---|---|---|---|
| **ASK** | Confirm scope, target date, readiness criteria, evidence available | `prompts/ask-clarifying-questions.md` | Question log |
| **PLAN** | Define readiness dimensions, evidence plan, blocker structure | `prompts/plan-scaffold.md` | Assurance plan |
| **CRITIQUE** | Pre-mortem: failure modes, overconfidence, missing dependencies | `prompts/red-team-critique.md` | Pre-mortem critique |
| **EXECUTE** | Draft delivery assurance summary | — | Delivery assurance summary |
| **REVIEW** | Verify readiness claims are evidenced | `prompts/evidence-gap-analysis.md` | Readiness verdict |
| **ITERATE** | Revise; coaching on go/no-go framing if needed | `prompts/leadership-coaching-challenge.md` | Revised summary |

---

## Evidence requirements

Minimum before EXECUTE:

- Readiness criteria explicitly stated
- Each readiness dimension linked to evidence or `[Unknown]`
- Blockers listed with owner **role** (not named individual), mitigation, and target date
- Rollback approach addressed or marked `[Unknown]`
- No "100% ready" language without defined criteria and supporting evidence

---

## Human checkpoints

- [ ] Leader confirms readiness criteria before EXECUTE
- [ ] Leader reviews pre-mortem critique
- [ ] **Go / no-go decision** recorded by leader in template — agent does not decide
- [ ] Conditional go conditions explicitly stated and accepted by leader

---

## Stop and refusal rules

**Stop and return to ASK if:**
- Readiness criteria are undefined
- Request focuses on individual accountability for delays

**Refuse:**
- False confidence language without evidence
- Go recommendation presented as agent decision

---

## Expected outputs

| Output | Template | Required |
|---|---|---|
| Delivery assurance summary | `templates/delivery-assurance-summary.md` | Yes |

Optional Mermaid readiness flow diagram within the summary.

---

## Example opening prompt for Cursor

```
@SOFTWARE_ENGINEERING_LEADER_AGENT.md @workflows/delivery-assurance.md

Delivery assurance for {programme/release} targeting {date}.
Audience: {who decides go/no-go}
Run full loop with emphasis on CRITIQUE and REVIEW.

Evidence: {test status, defects, dependencies, ops readiness}
Readiness criteria: {criteria}
```
