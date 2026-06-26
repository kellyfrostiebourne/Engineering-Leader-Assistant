# Prompt: Evidence Gap Analysis (REVIEW)

## Purpose

Verify audit-ready traceability before the leader treats an output as final. Identify unsupported claims, missing sources, and governance gaps.

## When to use

- Before finalising any material output
- Before external sharing (ExCo, Board, regulator, company-wide)
- After EXECUTE, as the primary REVIEW activity

## When not to use

- To fabricate evidence or plausible sources
- As a rubber-stamp when gaps are material — pause and return to ASK instead
- To assess individual performance

## Inputs required

- `{draft_artefact}` — the memo, brief, or HTML content under review
- `{topic}` — leadership question or scenario
- `{intended_audience}` — who will receive the final output
- `{sharing_context}` — internal only / ExCo / Board / external

---

## Prompt body

```
You are operating as the Software Engineering Leader Agent (REVIEW phase).

Topic: {topic}
Intended audience: {intended_audience}
Sharing context: {sharing_context}

Draft artefact to review:
{draft_artefact}

Perform an evidence gap analysis. Do not rewrite the full artefact unless correcting labels.

Produce:

1. **Claim inventory** — extract every material claim from the draft
2. **Traceability matrix** — map each claim to Evidence / Inference / Assumption / Unknown
3. **Gap list** — claims that cannot be supported; required actions
4. **Governance fit** — proportionate risk language; no false sign-off
5. **Confidentiality check** — confirm [REDACTED] applied where needed
6. **HTML consistency** (if applicable) — visual claims match evidence table
7. **Readiness verdict** — Ready / Ready with caveats / Not ready

For each gap, specify:
- Whether to obtain evidence, label [Unknown], label [Assumption], or remove the claim

Rules:
- Do not invent sources to close gaps
- Do not approve on behalf of the leader
- British English
- If Not ready, list minimum fixes before sharing
```

---

## Expected output shape

```markdown
## Evidence gap analysis — {topic}

### Claim inventory
| # | Claim | Location in draft |
|---|---|---|
| 1 | ... | ... |

### Traceability matrix
| # | Claim | Type | Source / basis | Confidence |
|---|---|---|---|---|
| 1 | ... | Evidence/Inference/Assumption/Unknown | ... | High/Medium/Low |

### Gaps requiring action
| # | Gap | Recommended action | Owner |
|---|---|---|---|
| 1 | ... | Obtain evidence / Label Unknown / Remove | Leader |

### Governance fit
...

### Readiness verdict
**Verdict:** Ready / Ready with caveats / Not ready
**Caveats:** ...
**Minimum fixes (if not ready):** ...
```

## Guardrails

- A verdict of "Ready" requires no High-severity unsupported claims for the intended audience
- For external sharing, any [Unknown] on material claims should trigger "Not ready" or explicit leader acceptance of caveats
- Never simulate audit or compliance sign-off
