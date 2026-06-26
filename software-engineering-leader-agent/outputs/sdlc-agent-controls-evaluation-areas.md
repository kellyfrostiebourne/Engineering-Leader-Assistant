# SDLC Agent Controls — Evaluation Areas (Strategy & Implementation)

| Field | Value |
|---|---|
| Date | 26 June 2026 |
| Author | Software Engineering Leader Agent (draft) |
| Audience | Engineering leadership; ExCo if escalated |
| Organisation | {Organisation} |
| Decision status | Draft — **Pending leader approval** |
| Version | 1.0 |
| Classification | Internal |

---

## Executive summary

- AI coding agents materially change the SDLC threat model: credentials, tool permissions, and unreviewed autonomous changes become primary control surfaces, not secondary concerns. `[Inference]`
- **Environment security and secret isolation** is the foundational control plane — without it, cost monitoring and quality gates provide false assurance. `[Inference]`
- **Agent usage monitoring** must unify cost and security telemetry; separating them creates blind spots when anomalous spend correlates with policy violations. `[Inference]`
- **Value efficiency** requires explicit guardrails on autonomy scope; unconstrained YOLO modes drive activity metrics (lines changed, sessions run) without outcome accountability. `[Inference]`
- **Prompt injection** is an SDLC supply-chain risk — untrusted inputs in issues, PRs, dependencies, docs, and MCP servers can redirect agent tool use; policy alone is insufficient without enforcement points. `[Evidence]` — widely documented class of LLM application risks (OWASP LLM Top 10: prompt injection)
- **Quality gates** must be embedded at orchestration boundaries, not only at merge/deploy — agents that bypass plan/review phases export risk downstream. `[Inference]`
- Regulated fintech context implies audit traceability from agent session → artefact → human review → release; ad hoc agent use without logging is likely unauditable. `[Inference]`
- Multi-agent orchestration amplifies "confused deputy" risk — one agent with elevated permissions can be steered by another agent or untrusted content. `[Inference]`

**Ask (pending your approval):** Prioritise which control domain to assess first against `{Organisation}`'s current state, and confirm whether agent use is **local IDE only**, **cloud agents**, or **both** — this determines the monitoring and secret-isolation architecture.

---

## Evaluation areas

### 1. Environment security and secret isolation

| Field | Content |
|---|---|
| **Control objective** | Agents operate with least privilege; cannot read, inject, or exfiltrate credentials, secrets, or production-adjacent configuration. Blast radius of compromise or misdirection is bounded and recoverable. |
| **Strategy evaluation** | Define risk appetite for agent runtime location (local vs cloud vs hybrid). Establish policy on: which environments agents may touch (dev only vs staging); prohibition on production credentials in agent context; approved agent platforms and MCP servers; secret handling standards; incident response when secrets are exposed to an agent session. Assign accountability: platform engineering (sandboxing), security (policy), engineering leaders (operating model enforcement). |
| **Implementation evaluation** | **Secret isolation:** `.env` / `.gitignore` enforcement; pre-commit and CI secret scanning (e.g. gitleaks, trufflehog); block agent access to secret stores without ephemeral scoped tokens. **Sandboxing:** containerised or VM-isolated cloud agent runtimes; read-only filesystem where possible; network egress allowlists. **Tool permissions:** MCP server scoping; Cursor/agent rule files that refuse credential requests; disable or gate shell commands that read env vars. **Runtime separation:** distinct cloud agent identities per team/environment; no shared long-lived API keys in agent config. **Blast radius:** short-lived tokens; automatic rotation on suspected exposure; kill switch for agent sessions. **Enforcement points:** IDE settings, agent platform admin controls, CI policy-as-code, pre-flight checks before agent session start. |
| **Key risks if uncontrolled** | Credential exfiltration via prompt injection or tool misuse; agents committing secrets to repos; lateral movement from dev credentials to staging/production; unauditable shadow use of personal API keys. `[Inference]` |
| **Indicators / signals** | Count of secret scan findings in agent-generated commits; agent sessions with access to paths containing `.env` or secret manager configs; MCP tool calls targeting credential files or env-var reads; network egress from agent runtime to unexpected destinations; incidents of secrets rotated due to agent exposure. |
| **Maturity questions** | 1. Can an agent session access any file containing credentials today? 2. Are cloud agents running with organisation-managed identities or personal API keys? 3. Is there an enforced deny-list for paths and tools? 4. What happens within 60 minutes if a secret is exposed to an agent? 5. Are MCP servers approved and scoped per use case? |
| **Candidate control options** | **Option A — Restrictive:** Agents dev-only; local IDE agents only; no MCP; all secrets via ephemeral OIDC; human approval before any network or shell tool use. Lower risk, slower delivery. **Option B — Balanced autonomy:** Approved cloud agent platform with isolated runtime; scoped MCP allowlist; secret scanning on all agent output; staging access with read-only production telemetry. Recommended for your consideration pending `{Organisation}` risk appetite. **Option C — Progressive:** Pilot Option A; expand to B with measured control effectiveness. |

---

### 2. Agent usage monitoring — cost and security

| Field | Content |
|---|---|
| **Control objective** | All material agent activity is observable, attributable, retainable, and alertable — for cost governance and security investigation — with evidence suitable for regulated engineering audit. |
| **Strategy evaluation** | Define what constitutes "material" agent use (e.g. autonomous multi-file changes, cloud agent sessions, MCP tool use). Establish cost ownership (team chargeback vs central budget). Set retention and access policy for agent logs (who can view, how long retained, GDPR/data handling). Align with security operations: which anomalies escalate to SecOps. Define approved metrics for leadership reporting vs operational alerting. |
| **Implementation evaluation** | **Telemetry to capture:** session ID, user/team, model, token in/out, tool invocations (type, target, outcome), repos/paths touched, duration, mode (Ask/Agent/Plan), cloud vs local, MCP servers used, network egress summary, cost estimate per session. **Cost governance:** per-team budgets; alerts at 80/100%; efficiency ratio (e.g. merged PRs per 1M tokens — define numerator/denominator, do not target fabricated values). **Security monitoring:** anomalous tool patterns (mass file delete, credential path access); sessions outside approved hours/geo; duplicate sessions on sensitive repos; prompt patterns matching injection heuristics. **Audit trail:** immutable log store; correlation ID from agent session → PR → deployment. **Integration:** SIEM export; dashboard for engineering leadership; monthly cost + security summary. |
| **Key risks if uncontrolled** | Unbounded spend; shadow AI use on sensitive codebases; inability to reconstruct incident timeline; regulatory challenge to demonstrate control operation. `[Inference]` |
| **Indicators / signals** | Token spend by team/week vs budget; sessions with zero merged outcome (activity without value); tool calls to denied paths; spike in cloud agent use on regulated systems; mean time to detect policy violation; log completeness (% sessions with full telemetry). |
| **Maturity questions** | 1. Can you attribute last month's agent spend to a team and initiative? 2. Can you reconstruct what an agent did in a session 30 days ago? 3. Are there alerts for anomalous tool use? 4. Is cloud agent use centrally provisioned or ad hoc? 5. Do audit requests for "who changed what via AI" have a defined evidence source? |
| **Candidate control options** | **Option A — Centralised observability:** Single approved agent platform; mandatory logging; no local-only shadow tools for regulated repos. **Option B — Federated with minimum bar:** Local IDE agents allowed with client-side logging agent and periodic export; cloud agents centralised. Minimum telemetry schema enforced org-wide. **Option C — Cost-only initially:** Implement spend caps first; add security alerting in phase 2 (accept interim risk — leader must explicitly accept). |

---

### 3. Value efficiency — agents building the right things well

| Field | Content |
|---|---|
| **Control objective** | Agent use measurably contributes to delivery outcomes (merged, reviewed, deployed value) rather than generating unreviewed activity, scope creep, or technical debt. |
| **Strategy evaluation** | Define when agent autonomy is permitted (bug fixes, tests, refactors within bounded scope) vs prohibited (architecture changes, security-sensitive modules, regulatory logic without human design). Set task-sizing norms: agents work from approved plans/tickets with acceptance criteria. Establish human review minimums before merge. Anti-pattern catalogue for teams (speculative refactors, framework rewrites, "YOLO" on main). Link agent use to outcome metrics in engineering health reviews — systems level, not individual scoring. |
| **Implementation evaluation** | **Scope guardrails:** Cursor rules / agent instructions per repo; max files/lines per autonomous session; require Plan mode or human-approved plan artifact before Agent mode on material changes. **Review enforcement:** mandatory human PR review for agent-generated code; block auto-merge on agent-labeled PRs; CODEOWNERS on sensitive paths. **Value signals:** ratio of agent-assisted PRs merged vs abandoned; cycle time vs rollback rate; defect escape rate on agent-touched modules (define measurement before claiming improvement). **Workflow integration:** agents invoked from linked tickets with defined done-criteria; prohibit drive-by agent sessions on production hotfix branches without incident commander approval. |
| **Key risks if uncontrolled** | High throughput of low-quality code; hidden technical debt; engineers disengage from understanding agent output; regulatory logic changed without domain expert review. `[Inference]` |
| **Indicators / signals** | Agent PR merge rate vs revert rate; review comments per agent PR vs human-only PR; sessions ending without PR; lines changed per merged outcome; rollback frequency within 7 days of agent-assisted deploy. |
| **Maturity questions** | 1. Is there a written autonomy matrix (what agents may do alone)? 2. Are agent-generated PRs identifiable and reviewed differently? 3. Do teams track outcomes or only session volume? 4. Has any agent session modified regulated calculation logic without design review? 5. Is YOLO/autonomous mode restricted by repo or branch? |
| **Candidate control options** | **Option A — Plan-gated autonomy:** No Agent mode without approved plan artifact; strict on regulated services. **Option B — Tiered repos:** Low restriction on internal tooling repos; high restriction on customer-facing/regulated codebases. **Option C — Pairing model:** Agent as drafter only; human executes merge decision after line-by-line review on sensitive paths. |

---

### 4. Prompt injection and adversarial use

| Field | Content |
|---|---|
| **Control objective** | Untrusted content cannot reliably redirect agent behaviour to exfiltrate data, misuse tools, or bypass instruction hierarchy — in single-agent and orchestrated multi-agent workflows. |
| **Strategy evaluation** | Treat prompt injection as an application security risk, not merely an AI policy issue. Define trust boundaries: which inputs are untrusted (issue bodies, PR descriptions, README, dependency docs, web fetch, MCP responses). Establish instruction hierarchy: system/org rules override user and retrieved content. Risk appetite for web-enabled agents and third-party MCP. Incident response playbooks for suspected injection (session termination, credential rotation, forensic log review). Training for engineers on adversarial prompts in supply chain. |
| **Implementation evaluation** | **Input sanitisation:** mark untrusted blocks; strip or quarantine hidden instructions in pasted content. **Tool boundaries:** agents cannot pass retrieved text directly into shell/git/credential tools without validation; separate "read untrusted" from "act with privilege" steps. **Multi-agent orchestration:** orchestrator agent has no elevated permissions; worker agents scoped per task; no privilege escalation via inter-agent messages. **Detection:** heuristics on tool-call sequences (read secret file after web fetch); human review triggers on high-risk patterns. **Supply chain:** dependency and MCP server vetting; pinned versions; review of MCP tool definitions. **Response:** kill session; alert SecOps; rotate exposed credentials; post-incident control review. |
| **Key risks if uncontrolled** | Data exfiltration via indirect prompt injection in tickets/docs; malicious MCP server returning injected instructions; orchestrated agents chaining tool misuse; social engineering via "ignore previous instructions" in PR comments. `[Evidence]` — OWASP LLM01 prompt injection; `[Inference]` for multi-agent amplification |
| **Indicators / signals** | Tool calls following untrusted content ingestion; sessions with web fetch + file write + network egress in sequence; MCP servers not on allowlist; frequency of blocked injection-pattern rules firing; security review findings tagged prompt-injection. |
| **Maturity questions** | 1. Are issue/PR/doc bodies treated as untrusted when fed to agents? 2. Can retrieved web content directly drive shell commands? 3. Are MCP servers allowlisted and reviewed? 4. In multi-agent flows, can a low-trust agent instruct a high-privilege agent? 5. Is there a tested incident response for suspected injection? |
| **Candidate control options** | **Option A — Minimal trust surface:** No web tools; MCP deny-by-default; human approves tool use per session on regulated repos. **Option B — Detect and respond:** Allow web/MCP with logging, heuristics, and mandatory review on triggered patterns. **Option C — Architectural separation:** Untrusted content summarised in read-only context; action agent runs in separate session with no retrieved text in system prompt. |

---

### 5. Quality gates embedded in agent-assisted SDLC

| Field | Content |
|---|---|
| **Control objective** | Agent-assisted work passes the same — or stricter — quality gates as human-only work, with traceability from agent output through human review to release. YOLO/autonomous modes cannot bypass gates. |
| **Strategy evaluation** | Define definition of done for agent-assisted changes. Specify non-negotiable gates: design/plan review for material change, peer code review, SAST/dependency scan, test coverage thresholds, security review for sensitive modules, deployment approval. Clarify human-in-the-loop minimums for regulated SDLC (what must never be agent-only). Map gates to agent operating modes (Ask/Plan/Agent/YOLO). Integrate with delivery assurance and engineering health workflows. |
| **Implementation evaluation** | **Gate placement:** Plan artifact required before autonomous code gen on material scope; CI blocks merge on scan/test failures (agent PRs not exempt); deployment pipelines unchanged — no agent-triggered prod deploy without human approval. **Traceability:** PR label `agent-assisted`; link to agent session log ID; plan document in ticket. **YOLO enforcement:** platform blocks YOLO on protected branches; time-boxed sessions; auto-stop on gate failure. **Regression risk:** mandatory tests for agent-touched modules; diff size limits triggering extra review. **Enforcement:** branch protection; required checks; CODEOWNERS; release management tooling. |
| **Key risks if uncontrolled** | Agents merge vulnerable or incorrect code at speed; audit cannot show who approved logic changes; YOLO mode bypasses design review; test gaps in agent-generated boilerplate. `[Inference]` |
| **Indicators / signals** | % agent PRs merging with failed/skipped checks (target: zero); agent PRs missing linked plan/ticket; gate bypass incidents; defect density in agent-assisted modules vs baseline (define baseline first — `[Unknown]` for `{Organisation}`); time from agent PR open to qualified human review. |
| **Maturity questions** | 1. Are agent PRs exempt from any CI checks today? 2. Is plan review required before large agent diffs? 3. Can agents merge to main without human approval? 4. Is there a PR label and session traceability standard? 5. Do release gates treat agent code differently from human code? |
| **Candidate control options** | **Option A — Strict parity plus:** Agent-assisted changes require all human gates plus plan artifact and security scan — no exceptions on regulated paths. **Option B — Risk-based gates:** Gate strictness scales by repo tier and change type; YOLO prohibited on tier-1 systems. **Option C — Agent-specific gate:** Additional automated review (e.g. LLM output diff analyser) as supplement, not substitute for human review — leader must not treat as compliance sign-off. |

---

## Cross-cutting themes

### Human accountability and human-in-the-loop minimums

For regulated SDLC at `{Organisation}`, the following should remain **human-owned** (agent may assist, not decide):

- Approval of material architecture and security design
- Acceptance of regulatory or customer-facing logic changes
- Production deployment and rollback decisions
- Exception to policy (e.g. temporary gate waiver)
- Sign-off for external or audit-facing statements

`[Assumption]` — aligned with responsible AI practice in regulated contexts; confirm against `{Organisation}` policy.

### Responsible AI alignment with agent orchestration

- Document agent use cases and prohibited use cases per system tier
- Maintain transparency: stakeholders can determine when AI contributed to a change
- No unverified claims of quality or security improvement from agent adoption
- Periodic review of agent control effectiveness (engineering health review lens — systems, not individuals)

### Integration with existing engineering practices

| Existing practice | Integration point |
|---|---|
| Engineering health review | Add agent control maturity and outcome metrics as a theme |
| Delivery assurance | Include agent-assisted change volume and gate compliance in readiness checks |
| Governance / risk decisions | Use decision record template for agent platform and autonomy policy choices |
| Audit | Agent session logs + PR traceability as evidence of control operation |

---

## Red-team critique (embedded)

Top 5 failure modes if controls are poorly designed or treated as checkbox compliance:

1. **Secret isolation theatre** — Secret scanning on commit but agents read `.env` at runtime; logs show no exposure because exfiltration went via MCP network tool. `[Inference]`

2. **Cost dashboards without security correlation** — Teams celebrate token efficiency while anomalous tool use on sensitive repos goes unalerted. `[Inference]`

3. **YOLO mode on protected paths** — Policy says "plan first"; platform allows unconstrained Agent mode on main branch for speed. `[Inference]`

4. **Injection via "trusted" channels** — Jira ticket or Confluence page treated as trusted context; attacker or compromised dependency doc redirects agent to leak credentials. `[Inference]`

5. **Quality gates that agents circumvent** — Agent opens PR; human rubber-stamps without reading; CI checks waived for "small" agent diffs that aggregate to material change. `[Inference]`

**Proceed recommendation:** Proceed with phased assessment — prioritise domains 1 and 5 (secret isolation and quality gates) before scaling agent autonomy. **Pending leader approval.**

---

## Evidence and assumptions

### Evidence table

| # | Claim | Type | Source / basis | Confidence |
|---|---|---|---|---|
| 1 | Prompt injection is a primary LLM application risk class | [Evidence] | OWASP Top 10 for LLM Applications (LLM01) | High |
| 2 | Agent tool use expands attack surface beyond chat | [Inference] | Agent architectures with shell, file, network, MCP tools | Medium |
| 3 | Regulated engineering requires traceability from change to approval | [Inference] | Common audit expectations in financial services engineering | Medium |
| 4 | Unmonitored agent use creates cost and security blind spots | [Inference] | Operational risk from ungoverned SaaS/API consumption patterns | Medium |
| 5 | Multi-agent orchestration increases confused-deputy risk | [Inference] | Privilege delegation patterns in distributed systems | Medium |
| 6 | Quality gates must apply equally to agent-generated code | [Assumption] | `{Organisation}` SDLC parity expectation — confirm | Medium |
| 7 | `{Organisation}` uses or plans cloud and local coding agents | [Unknown] | Requires leader input | — |
| 8 | Current MCP server inventory and approval process | [Unknown] | Requires leader input | — |
| 9 | Current CI/CD gate configuration for agent PRs | [Unknown] | Requires leader input | — |
| 10 | Existing secret management and rotation standards | [Unknown] | Requires leader input | — |

### Assumptions

- [Assumption] `{Organisation}` operates under financial services regulatory expectations for engineering change control.
- [Assumption] Engineering teams are adopting or piloting AI coding agents (Cursor or equivalent) in the next 12 months.
- [Assumption] Human code review remains the primary control for logic correctness on regulated paths.

### Unknowns (require `{Organisation}` input)

- [Unknown] Approved agent platforms (Cursor local, Cursor cloud agents, other)
- [Unknown] Current secret storage model (Vault, cloud SM, env files in dev)
- [Unknown] MCP servers in use and approval process
- [Unknown] Whether agents can access staging or production-adjacent systems today
- [Unknown] Existing SIEM/logging integration for developer tooling
- [Unknown] Current PR policies and branch protection on regulated repositories
- [Unknown] Risk appetite for YOLO/autonomous modes by system tier
- [Unknown] Budget ownership for AI/agent spend

---

## Questions for leader review

1. Which agent platforms are approved or in pilot at `{Organisation}` (local IDE, cloud agents, CI-integrated agents)?
2. What is the system tiering model (e.g. tier-1 customer/regulatory vs internal tooling) for autonomy rules?
3. Can you supply current branch protection and required CI checks on tier-1 repos?
4. What is the secret management standard — and are `.env` files present in developer workspaces agents can read?
5. Is there an MCP allowlist? Which MCP servers are in use?
6. What logging exists today for Cursor/agent sessions — if any?
7. What is your risk appetite: Option A restrictive, Option B balanced, or phased Option C?
8. Who owns agent governance: platform engineering, security, or a joint working group?
9. Is there a target date for ExCo or audit visibility on agent controls?
10. Which control domain should be baselined first with a maturity assessment?

---

## Optional next steps

| Follow-on | Workflow | Purpose |
|---|---|---|
| Deep-dive on Cursor/cloud agent platform controls | `workflows/technical-research-brief.md` | Evidence-backed options paper on a specific platform |
| Pilot readiness for agent-assisted delivery on one team | `workflows/delivery-assurance.md` | Gate compliance and rollback plan before scaling |
| ExCo briefing on agent risk and control roadmap | `workflows/strategy-synthesis.md` + `templates/executive-briefing-note.md` | Executive narrative once you approve prioritisation |
| Quarterly agent control health check | `workflows/engineering-health-review.md` | Systems-level monitoring of control effectiveness |
| Formal autonomy policy decision | `workflows/governance-risk-decision.md` | Decision record for org-wide agent operating model |

**Optional HTML one-pager:** If you approve prioritisation, a `leadership-visual-summary.html` companion could summarise the five control domains for ExCo — not produced in this pass.

---

## Human decision required

- [ ] I have reviewed evidence, assumptions, and unknowns
- [ ] I prioritise control domain(s) for baseline assessment: _______________
- [ ] I select preferred control posture: Restrictive / Balanced / Phased — **Pending leader approval**
- [ ] I approve escalation to ExCo / implementation planning: Yes / No / With caveats

**Leader signature / date:** _______________

---

## Review checklist (agent self-review — REVIEW phase)

- [x] No named individual evaluation
- [x] Material claims labelled Evidence / Inference / Assumption / Unknown
- [x] No `{Organisation}`-specific policies or audit outcomes invented
- [x] Recommendations framed for leader consideration; status Pending leader approval
- [x] British English throughout
- [x] Five control domains covered with strategy and implementation evaluation
- [x] Questions for leader review captured instead of blocking ASK phase

**Readiness verdict:** Ready with caveats — suitable for internal leadership review; **Not ready** for ExCo until unknowns in §Questions for leader review are addressed.

**Status:** All recommendations **Pending leader approval**
