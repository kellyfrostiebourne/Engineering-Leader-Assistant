# SDLC Agent Controls — What to Evaluate (Leader View)

| Field | Value |
|---|---|
| Date | 26 June 2026 |
| Audience | Senior software engineering leader |
| Organisation | {Organisation} |
| Status | Draft — Pending leader approval |
| Current agent scope | Local IDE agents on individual Macs only — no org cloud agents today |
| Current secret exposure | Local secrets on developer Macs only (e.g. env files, local credentials) |

---

## Executive summary

- AI coding agents change how software gets built — and where risk sits. Today, that risk is **on the developer Mac**, not in a central cloud agent platform.
- Agents can already reach **local secrets** on individual machines — env files, CLI credentials, API keys in workspaces. Blast radius is **per developer**, but still material if those secrets reach staging or internal services.
- Central cloud controls can wait until you adopt cloud agents. **Endpoint hygiene and quality gates cannot.**
- Local agent sessions are **harder to see centrally** — git, PR labels, and CI become your primary audit trail until monitoring matures.
- Spend on agents can grow quietly on personal subscriptions; without visibility, you will not spot misuse or waste until it hurts.
- The biggest delivery risk is not slow adoption — it is **fast, ungoverned output** that bypasses design review and quality gates.
- Untrusted content in tickets, documents, and third-party integrations can steer agents off course — treat this as a supply-chain problem, not a policy paragraph.
- You do not need a perfect framework on day one. You need a **clear evaluation sequence**, named owners, and a chosen posture for autonomy.

**Ask:** Approve the 90-day priority sequence below, make the three decisions this week, and assign owners to baseline the **Now** items before scaling agent use further.

---

## The ask (pending your approval)

### 90-day priority sequence

| Horizon | Domain | Focus |
|---|---|---|
| **Now** (0–30 days) | 1. Environment security and secret isolation | **Known:** agents can reach local Mac secrets. Baseline workspace hygiene, env file rules, and commit leakage — not cloud sandboxing yet. |
| **Now** (0–30 days) | 5. Quality gates in agent-assisted SDLC | Do agent-assisted changes pass the same gates as human work? |
| **Next** (30–60 days) | 2. Agent usage monitoring — cost and security | Local sessions lack central telemetry — define minimum visibility via PR traceability, labelling, and spend governance first. |
| **Next** (30–60 days) | 4. Prompt injection and adversarial use | Where does untrusted content enter agent workflows? |
| **Later** (60–90 days) | 3. Value efficiency — building the right things well | Are agents delivering outcomes, not just activity? |

### Three decisions needed this week

1. **Control posture:** Adopt **Balanced for today** (recommended) — strict endpoint and gate controls on regulated repos; defer cloud agent controls until adoption. Or choose Restrictive / Progressive instead.
2. **Scope of immediate baseline:** **Confirmed for today:** local IDE agents on Mac only. **Decision needed:** define the control bar **before** cloud agents are switched on.
3. **Accountability model:** Confirm joint ownership — **Engineering Leadership** (developer workflow and gates), **Platform Engineering** (tooling standards), **Security** (policy and monitoring).

### Suggested owners by role

| Domain | Primary owner | Supporting |
|---|---|---|
| 1. Secret isolation (Mac endpoint) | Engineering Leadership | Platform Engineering, Security |
| 2. Usage monitoring | Platform Engineering | Security, Engineering Leadership |
| 3. Value efficiency | Engineering Leadership | Delivery leads |
| 4. Prompt injection | Security | Platform Engineering |
| 5. Quality gates | Engineering Leadership | Platform Engineering, Security |

### If we do nothing for six months

Ungoverned **local** agent use will spread — secrets in project folders, personal API keys, uneven review standards, and no central session history. When something goes wrong, you will struggle to reconstruct which agent session touched which change. You may adopt cloud agents later without a baseline — repeating the same gaps at org scale. Expect a reactive ban or freeze rather than a measured operating model.

---

## Five evaluation areas

### 1. Keeping local secrets off the agent’s reach (Mac endpoint)

**Why this matters to the business**  
Today, agents run on **individual Macs** and can read the developer’s workspace — including local env files and credentials stored for convenience. You are not facing org-wide cloud agent compromise yet; you **are** facing repeated per-machine exposure if secrets live where agents work. In fintech, even dev or staging keys can reach sensitive systems.

**What good looks like**  
No secrets in agent-readable project paths. Developers use safer patterns for local credentials. Agents cannot casually print environment variables or read credential files. Leaked secrets are caught before merge — not after.

**What I should evaluate strategically**

- Which local secret patterns exist today — env files, CLI config, keys in repos?
- Can agent shell tools expose environment variables or credential files?
- Do regulated repos have stricter workspace rules than internal tooling repos?
- What is the blast radius of one compromised local session — which systems could those dev secrets reach?
- Who sets and audits developer Mac hygiene standards — not individual blame, but the norm?

**Who should own the deep dive**  
Engineering Leadership (lead), Platform Engineering and Security (supporting)

**How we’ll know it’s working**

- Documented standard: no secrets in project directories on tier-one repos — spot-checked, not policed individually
- Secret scanning catches agent-generated commits before merge — count tracked, target agreed by you
- Reduced incidence of credential files in workspaces over time — baseline then trend
- Clear runbook when a local secret may have entered an agent context — rotation steps defined

---

### 2. Seeing what agents do — cost and security together

**Why this matters to the business**  
Local IDE agents are **invisible to central platforms** by default. You cannot govern what you cannot see. Personal subscriptions and offline sessions still create cost and risk. Until cloud agents arrive, your best audit trail may be **what entered git** — not what happened on the Mac.

**What good looks like**  
Every material agent-assisted change is labelled and traceable through PR and CI to release. Spend is visible at team level. When cloud agents are adopted later, central telemetry is already specified — not bolted on after an incident.

**What I should evaluate strategically**

- Can you tell which merged changes were agent-assisted — from PR metadata alone?
- Is there a trail from pull request to ticket to release for agent work?
- How is agent spend captured today — personal expenses or central licences?
- What minimum session or change metadata should engineers record until central logging exists?
- What telemetry bar must be met **before** cloud agents are approved?

**Who should own the deep dive**  
Platform Engineering (lead), Engineering Leadership and Security (supporting)

**How we’ll know it’s working**

- Agent-assisted PRs labelled consistently — percentage tracked monthly
- Team-level agent spend visible — personal or central — against agreed budget
- Documented minimum traceability standard for local agent sessions (what to capture, where)
- Cloud-agent readiness checklist drafted — even if cloud is not in use yet

---

### 3. Agents that build value — not just volume

**Why this matters to the business**  
Agents can produce code faster than teams can absorb it. Without guardrails, you get activity — lines changed, sessions run — without outcomes. Technical debt and review fatigue follow. Regulated logic can change without the right human eyes.

**What good looks like**  
Agent use is tied to clear work items with acceptance criteria. Autonomy is allowed where risk is low and reversed where it is not. Leaders see outcomes — merged, reviewed, stable delivery — not token counts.

**What I should evaluate strategically**

- Is there a written map of where agent autonomy is allowed — and forbidden?
- Are agent-assisted changes identifiable in your delivery workflow?
- Do teams measure outcomes from agent use — or only usage volume?
- Where must a human design or approve before an agent acts?
- Is unconstrained autonomous mode restricted on your most critical systems?

**Who should own the deep dive**  
Engineering Leadership (lead), Delivery and platform leads (supporting)

**How we’ll know it’s working**

- Ratio of agent-assisted changes merged vs abandoned (defined and tracked)
- Rollback rate within seven days of agent-assisted releases (vs baseline you set)
- Percentage of agent work linked to tickets with acceptance criteria
- No autonomous changes to tier-one regulated logic without recorded human approval

---

### 4. When untrusted content steers the agent

**Why this matters to the business**  
Agents do not only listen to you. They ingest ticket text, pull requests, documentation, dependency readmes, web pages, and third-party tool responses. Attackers — or compromised supply chain content — can embed instructions that redirect privileged tools. Multi-agent setups multiply this risk.

**What good looks like**  
Untrusted inputs are treated as untrusted. High-privilege actions require separation from raw retrieved content. You have detection, response, and recovery — not just a slide that says “don’t paste untrusted text.”

**What I should evaluate strategically**

- Which inputs are treated as untrusted in your agent workflows today?
- Can web or third-party tool content flow directly into privileged actions?
- Are third-party agent integrations approved and scoped — or installed ad hoc?
- In multi-agent flows, can a low-trust step instruct a high-privilege step?
- Do you have a tested response when injection is suspected?

**Who should own the deep dive**  
Security (lead), Platform Engineering (supporting)

**How we’ll know it’s working**

- Allowlist of approved third-party agent integrations — reviewed quarterly
- Alerts on high-risk tool sequences after untrusted content ingestion
- Incident response playbook exercised at least once (tabletop acceptable)
- Engineers trained on adversarial content in tickets and docs — uptake tracked

---

### 5. Quality gates that still hold when agents go fast

**Why this matters to the business**  
Speed without gates is how defects, vulnerabilities, and unapproved logic reach production. Agents — especially in autonomous modes — can skip the thinking and review phases humans naturally pause at. Your SDLC gates must assume agents, not exempt them.

**What good looks like**  
Agent-assisted work passes the same — or stricter — checks as human work. Every material change links back to a plan, a reviewer, and a record. Autonomous modes cannot bypass protected branches or release approval.

**What I should evaluate strategically**

- Are agent-assisted pull requests exempt from any existing checks?
- Is plan or design review required before large autonomous changes?
- Can agents merge or deploy without human approval on critical paths?
- Is there a standard way to label and trace agent-assisted work?
- Do release gates treat agent-generated code differently — or the same?

**Who should own the deep dive**  
Engineering Leadership (lead), Platform Engineering and Security (supporting)

**How we’ll know it’s working**

- Zero merges to protected branches with skipped required checks
- Agent-assisted changes carry trace IDs linked to session records
- Plan artifact present for material autonomous work — audited by sample monthly
- Defect escape rate on agent-touched modules tracked against your baseline

---

## Recommended control posture

**Recommended for your consideration: Balanced for today — endpoint-first**

Given **local Mac agents only** and **known local secret exposure**, prioritise:

- **Endpoint and workspace rules now** — no secrets in agent paths; stricter rules on tier-one repos; secret scanning on commits
- **Quality gates now** — agent-assisted changes treated like any other change; no autonomous mode on protected branches
- **PR-led traceability now** — label agent-assisted work; link to tickets; CI as audit evidence
- **Cloud controls later** — define sandboxing, org identities, and central logging **before** cloud agents are enabled

**Alternative:** Restrictive — local agents on internal repos only; no third-party integrations; no autonomous mode on regulated codepaths. Matches current reality closely; slower delivery on tier-one work.

**When cloud agents are adopted:** extend to full Balanced — org-provisioned runtimes, integration allowlist, unified telemetry. Do not enable cloud until endpoint baseline and gate compliance are in place.

**Status:** Pending leader approval

---

## ExCo readiness note

This topic is not ExCo-ready until you can answer: local agent standards are baselined, **Now** assessments on Mac secret hygiene and quality gates are complete, and cloud-agent criteria are defined even if cloud is not live. Part 2 supports delegate work; an ExCo briefing should wait until tier-one gate gaps and local secret patterns are understood.

---

# SDLC Agent Controls — Appendix (Strategy & Implementation Detail)

**Status:** Pending leader approval  
**Audience:** Platform Engineering, Security, Engineering Leadership delegates

---

## Glossary

| Term | Definition |
|---|---|
| **Agent** | AI system that can plan and execute actions — edit code, run commands, call tools — not just answer chat |
| **Autonomous / YOLO mode** | Agent executes multi-step work with minimal mid-flight human confirmation |
| **MCP** | Model Context Protocol — standard for connecting agents to external tools and data sources |
| **Prompt injection** | Untrusted text that manipulates agent behaviour — in tickets, docs, web content, or tool responses |
| **Tier-one systems** | Customer-facing or regulated logic paths — highest control strictness |
| **Mac endpoint scope** | Current state: agents run locally on developer Macs; secrets reachable are local only today |
| **Quality gate** | Mandatory checkpoint — plan review, code review, tests, security scan, release approval |

---

## Domain 1 — Environment security and secret isolation

### Current state at `{Organisation}`

- **Agent runtime:** local IDE agents on individual Macs only — no org cloud agents today
- **Secret exposure:** local secrets on developer Macs (env files, local CLI credentials, API keys in workspaces)
- **Implication:** controls must prioritise **endpoint and workspace hygiene** before cloud sandboxing

### Strategy evaluation

**Today (Mac endpoint):**

- Developer Mac standards: where secrets may and may not live during agent use
- Policy on env files and local credentials in project directories
- Tier-one repo workspace rules vs internal tooling repos
- Incident response when a local secret may have entered an agent session (rotation, scope assessment)
- Accountability: Engineering Leadership (norms), Platform (tooling), Security (standards)

**Before cloud agents are enabled (future):**

- Risk appetite for cloud vs local vs hybrid
- Prohibition on production credentials in any agent context
- Approved platforms and third-party integrations
- Org-managed identities — no personal API keys on regulated work

### Implementation evaluation

**Today (Mac endpoint):**

- No secrets in project paths; documented alternative patterns for local dev credentials
- Pre-commit and CI secret scanning on all agent-assisted commits
- Agent instruction rules refusing credential access requests on tier-one repos
- Limit shell tool scope where platform allows — especially env-var and credential file reads
- Engineer guidance: what to remove from workspace before Agent mode on sensitive repos

**Before cloud agents (future — define now, implement when adopted):**

- Isolated cloud runtimes; network egress controls
- Org-managed agent identities; short-lived tokens
- Platform admin controls and session kill switch
- Pre-flight checks before cloud agent session start

### Key risks if uncontrolled

**Today:** local credential exposure; secrets committed to repos; staging or internal API access via dev keys on Mac; inconsistent developer hygiene.

**Future (if cloud enabled without baseline):** org-wide credential exposure; unauditable cloud sessions; repeating endpoint mistakes at scale.

### Maturity questions

**Today:**

1. Can an agent session read env files or credential paths in the workspace today? *(Known: yes — local Mac secrets)*
2. Which local secret patterns are in use across teams?
3. Are tier-one repos subject to stricter workspace rules?
4. What happens within 60 minutes if a local secret entered an agent session?
5. Are third-party integrations on Mac agents approved and scoped?

**Before cloud adoption:**

6. Cloud readiness criteria defined before switch-on?
7. Org-managed identity model agreed?

### Candidate control options

| Option | Summary |
|---|---|
| **A — Restrictive (fits today)** | Local agents only; no secrets in workspace; no third-party integrations on tier-one; strict gate on regulated paths |
| **B — Balanced (target when cloud added)** | Endpoint rules now; add org cloud platform with sandboxing and telemetry when adopted |
| **C — Progressive** | Enforce A on tier-one immediately; lighter rules on internal repos; cloud bar documented for later |

---

## Domain 2 — Agent usage monitoring — cost and security

### Current state at `{Organisation}`

- Local IDE agents on Macs — **limited central session telemetry** today
- Primary audit evidence likely via **PR metadata, git history, and CI** until central logging is introduced
- Cloud cost dashboards are **not applicable yet** — focus on licence/spend visibility and PR traceability first

### Strategy evaluation

- Definition of “material” agent use warranting traceability (agent-assisted PRs on tier-one repos as minimum)
- Cost ownership: personal subscriptions vs central licences — visibility requirement
- Minimum metadata engineers record for local sessions until platform logging exists
- **Cloud-agent telemetry requirements** documented now — implement when cloud is adopted
- Escalation path to security operations for anomalies detectable from git/CI signals

### Implementation evaluation

**Today (local agents):**

- PR labelling standard for agent-assisted changes
- Ticket linkage requirement for material agent work
- Team-level spend tracking — central licence or expense reporting
- CI and git as audit trail: who merged what, when, with which checks
- Sampling review: spot-check agent-assisted PRs on tier-one repos monthly

**When cloud agents are adopted:**

- Full telemetry: session ID, user/team, model, tokens, tool calls, repos/paths, duration, mode, integration use, cost estimate
- Centralised logging export; leadership dashboard; security alerting on anomalous tool patterns
- Correlation ID session → change request → release

### Key risks if uncontrolled

Unbounded spend; shadow use on sensitive code; inability to reconstruct incidents; audit challenge on control operation.

### Maturity questions

1. Are agent-assisted PRs labelled and linked to work items?
2. Can you attribute agent spend to teams today?
3. Is there a written minimum traceability standard for local sessions?
4. Are cloud-agent telemetry requirements defined before adoption?
5. Defined evidence source for “who changed what via AI” from git/CI?

### Candidate control options

| Option | Summary |
|---|---|
| **A — PR-led traceability (fits today)** | Label agent PRs; ticket linkage; CI/git as audit trail; team spend reporting |
| **B — Federated minimum bar** | A now; add client logging export if available; cloud telemetry when adopted |
| **C — Cloud-first (defer)** | Wait for cloud agents before monitoring investment — **not recommended**; gap persists on local use today |

---

## Domain 3 — Value efficiency — building the right things well

### Strategy evaluation

- Autonomy matrix: permitted vs prohibited work types
- Task sizing: agents work from approved tickets with acceptance criteria
- Human review minimums before merge
- Anti-pattern catalogue: speculative refactors, unreviewed autonomous changes on main
- Link to engineering health reviews — systems metrics, not individual scoring

### Implementation evaluation

- Scope guardrails: per-repo agent rules; limits on autonomous change size; plan required before material Agent mode
- Review enforcement: mandatory human review; block auto-merge on agent-labelled changes; ownership rules on sensitive paths
- Value signals: merge vs abandon rate; cycle time vs rollback; defect escape on agent-touched modules
- Workflow: agents invoked from linked work items; no drive-by autonomous work on hotfix branches without incident lead approval

### Key risks if uncontrolled

High-volume low-quality code; hidden debt; disengaged reviewers; regulated logic changed without domain expert review.

### Maturity questions

1. Written autonomy matrix exists?
2. Agent-assisted changes identifiable in workflow?
3. Outcomes tracked — not just session volume?
4. Regulated logic ever changed without design review?
5. Autonomous mode restricted by repo or branch tier?

### Candidate control options

| Option | Summary |
|---|---|
| **A — Plan-gated** | No autonomous mode without approved plan; strict on tier-one |
| **B — Tiered repos** | Light touch internal; heavy touch customer/regulated |
| **C — Drafter only** | Agent proposes; human merges after line review on sensitive paths |

---

## Domain 4 — Prompt injection and adversarial use

### Strategy evaluation

- Treat injection as application security, not AI policy alone
- Trust boundaries for untrusted inputs
- Instruction hierarchy: org rules override retrieved content
- Risk appetite for web-enabled agents and third-party integrations
- Incident response: terminate session, rotate credentials, forensic review
- Engineer awareness training

### Implementation evaluation

- Input handling: mark untrusted blocks; quarantine hidden instructions
- Tool boundaries: separate read-untrusted from act-with-privilege steps
- Multi-agent: orchestrator without elevated permissions; no privilege escalation via inter-agent messages
- Detection: heuristics on tool-call sequences; human review triggers
- Supply chain: integration vetting; pinned versions; review of tool definitions
- Response: kill session; alert security; rotate exposed credentials

### Key risks if uncontrolled

Data exfiltration via ticket/doc injection; malicious integration responses; chained tool misuse in multi-agent flows.

### Maturity questions

1. Ticket/PR/doc bodies treated as untrusted?
2. Can web content drive privileged commands directly?
3. Integrations allowlisted and reviewed?
4. Multi-agent privilege escalation possible?
5. Tested injection incident response?

### Candidate control options

| Option | Summary |
|---|---|
| **A — Minimal trust surface** | No web tools; deny-by-default integrations; human approves tools on tier-one |
| **B — Detect and respond** | Allow with logging, heuristics, mandatory review on triggers |
| **C — Architectural separation** | Summarise untrusted content in read-only context; action agent in separate session |

---

## Domain 5 — Quality gates in agent-assisted SDLC

### Strategy evaluation

- Definition of done for agent-assisted changes
- Non-negotiable gates: plan review, peer review, static analysis, tests, security review, deployment approval
- Human-in-the-loop minimums for regulated paths
- Map gates to agent modes (Ask / Plan / Agent / autonomous)
- Integration with delivery assurance and engineering health practices

### Implementation evaluation

- Gate placement: plan before large autonomous scope; CI blocks on failures; no agent-triggered prod deploy without human approval
- Traceability: label agent-assisted changes; link to session record; plan in work item
- Autonomous mode enforcement: block on protected branches; time-box sessions; stop on gate failure
- Regression: mandatory tests for touched modules; large diffs trigger extra review
- Enforcement: branch protection; required checks; ownership rules; release tooling

### Key risks if uncontrolled

Vulnerable code merged at speed; audit gap on who approved logic; autonomous mode bypasses design review; test gaps in generated boilerplate.

### Maturity questions

1. Agent changes exempt from any CI checks?
2. Plan review required before large autonomous diffs?
3. Agents can merge to main without human approval?
4. Standard label and session traceability?
5. Release gates same for agent and human code?

### Candidate control options

| Option | Summary |
|---|---|
| **A — Strict parity plus** | All human gates plus plan artifact on regulated paths — no exceptions |
| **B — Risk-based** | Gate strictness scales by repo tier; autonomous prohibited on tier-one |
| **C — Supplementary automated review** | Extra automated diff review — supplement only, not substitute for human review |

---

## Cross-cutting themes

### Human accountability

These remain human-owned — agents may assist, not decide:

- Material architecture and security design approval
- Acceptance of regulatory or customer-facing logic changes
- Production deployment and rollback
- Policy exceptions
- External or audit-facing statements

### Responsible AI alignment

- Document permitted and prohibited agent use cases by system tier
- Transparency when AI contributed to a change
- No unverified claims of quality or security improvement from adoption
- Periodic control effectiveness review via engineering health lens

### Integration with existing practices

| Practice | Integration |
|---|---|
| Engineering health review | Agent control maturity and outcome metrics as a theme |
| Delivery assurance | Gate compliance and agent-assisted change volume in readiness checks |
| Governance decisions | Decision record for platform and autonomy policy |
| Audit | Session logs plus change traceability as control operation evidence |

---

## Red-team critique — top 5 failure modes

1. **Secret scanning on commit only** — agents read local Mac secrets at runtime; commit scanning misses exfiltration via shell or network tools during the session.
2. **Cost dashboards without security correlation** — teams optimise tokens while anomalous tool use on sensitive repos goes unalerted.
3. **Policy says plan first; platform allows autonomous mode on main** — gap between policy and enforcement.
4. **Trusted ticket or wiki content** — injection via work management or documentation redirects privileged tools.
5. **Rubber-stamp review** — agent opens large pull request; human approves without reading; gates exist on paper only.

---

## Evidence and assumptions

### Evidence table

| # | Claim | Type | Basis | Confidence |
|---|---|---|---|---|
| 1 | Prompt injection is a documented LLM application risk class | Evidence | Industry security guidance on LLM applications | High |
| 2 | Agent tool use expands attack surface beyond chat | Inference | Architectures with shell, file, network, and integration tools | Medium |
| 3 | Regulated engineering expects change-to-approval traceability | Inference | Common financial services audit expectations | Medium |
| 4 | Ungoverned agent use creates cost and security blind spots | Inference | Pattern from ungoverned API and SaaS consumption | Medium |
| 5 | Multi-agent flows increase delegated-privilege risk | Inference | Privilege delegation in distributed systems | Medium |
| 6 | Tier-one systems require stricter gates for agent work | Assumption | `{Organisation}` risk tiering — confirm | Medium |
| 7 | Agent use is local IDE on Mac only today; no org cloud agents | Evidence | Leader-provided current state | High |
| 8 | Agents can reach local secrets on developer Macs today | Evidence | Leader-provided current state | High |
| 9 | Current branch protection and CI gate configuration | Unknown | Leader input required | — |
| 10 | Which local secret patterns exist (env files, CLI creds, etc.) | Unknown | Baseline assessment required | — |
| 11 | Existing logging for developer agent tooling | Unknown | Likely minimal for local IDE — confirm | — |

### Assumptions (Appendix)

- `{Organisation}` operates under financial services regulatory expectations for change control.
- Engineering teams use local IDE agents on Macs today; cloud agents are not org-provisioned yet.
- Local dev credentials may reach staging or internal services — blast radius per developer, not org-wide cloud.
- Human code review remains primary for logic correctness on regulated paths.

### Unknowns requiring `{Organisation}` input

- Which local secret patterns are in use (env files, CLI config, keys in repos)
- System tiering model for autonomy rules
- Current branch protection and required checks on tier-one repos
- Third-party integration inventory on local Mac agents
- Whether local dev credentials can reach staging or production-adjacent systems
- Agent spend model — personal subscriptions vs central licences
- **Cloud-agent control bar** — criteria before cloud is enabled (define now even if not live)
- Risk appetite for autonomous modes by repo tier

---

# Questions for leader review

1. **Posture:** Do you approve **Balanced for today (endpoint-first)** — or prefer Restrictive / Progressive?
2. **Local secrets:** Which local secret patterns are in use — env files, CLI credentials, keys in repos? Can those credentials reach staging or internal production-adjacent systems?
3. **Tiering:** What is `{Organisation}`’s system tier model — and which repos are tier-one?
4. **Accountability:** Do you confirm Engineering Leadership + Platform + Security as the joint ownership model?
5. **Cloud bar:** What conditions must be met before org cloud agents are allowed — even if not planned imminently?
6. **Target date:** When do you need enough confidence to brief ExCo — if at all in the next quarter?
7. **Evidence pack:** Can you supply current branch protection rules and one example of agent use on a tier-one repo?

---

**Document status:** All recommendations Pending leader approval  
**Version:** 2.1 (Updated for local Mac agents and local secret exposure only)
