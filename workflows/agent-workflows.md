# Agent Workflow Reference

**Owned by:** CoS (Chief of Staff)

*This document defines how agents are spawned and how workflows are orchestrated. Main (OWL) spawns CoS. CoS spawns and manages all other agents. CoS does not touch documents owned by others — each agent owns their own artifacts.*

---

## How Agents Are Spawned

**Main (OWL)** spawns **CoS**. That's it. CoS handles everything else.

**CoS spawns agents** in response to triggers (defined below). When CoS spawns an agent, CoS provides:
- The agent's workflow reference (this file + the agent's specific `workflow.md`)
- Reporting structure and roll-up chain
- Initial resource allocation (based on CRO data)
- Any relevant context from the trigger event

**Domain leads spawn sub-agents** under their allocation:
- **COO** spawns Engineering, Production, and Logistics (for new projects)
- **CSO** spawns PLMs (for new product lines)
- **Engineering** spawns ephemeral sub-agents (for sprint execution)

---

## Project Lifecycle

### Phase 1: Project Planning Session
**Who triggers:** CSO or PLM

**CSO initiates** when a new product is identified and CEO approves creation.

**PLM initiates** when market research indicates a significant gap between existing capabilities and desired capabilities, not covered under an existing project.

**Pre-session preparation:**
- **CSO** prepares: strategic context, competitive landscape, target market definition
- **PLM** prepares: product gap analysis, user persona data, proposed feature set
- **COO** prepares: operational capacity assessment, known constraints

**During the session (CSO + PLM + COO):**
1. Review strategic alignment and product gap
2. Define project scope (in-scope / out-of-scope)
3. Draft project charter using template at `abe/docs/charters/project-charter-template.md`
4. Identify preliminary resource needs and risks
5. Determine if cash is required (triggers CEO approval)

**Output:** Draft project charter

---

### Phase 2: Charter Review & Approval
**Who triggers:** CoS circulates the draft charter for approval

**Review sequence:**
1. **CFO** reviews: resource estimates, budget impact, cash requirements
2. **GC** reviews: licensing implications, IP considerations, contract needs
3. **CSO + COO** co-approve: strategic alignment and operational feasibility
4. **CEO** approves: required if cash is required; optional but recommended for strategic projects

**On approval, CoS notifies COO to spawn:**
- **Engineering** (always) — plans and executes the project
- **Logistics** (if needed) — supply chain, procurement, vendor management
- **Production** (if needed) — deployment, release coordination

---

### Phase 3: Engineering Onboarding (Sprint Zero)
**Who triggers:** Engineering, after being spawned

Before sprint planning, Engineering conducts a Sprint Zero:
1. **Codebase review** — understand existing architecture, patterns, and technical debt
2. **Environment setup** — ensure dev/staging/production environments are ready
3. **Story estimation** — validate and refine story estimates from the charter
4. **Dependency mapping** — identify cross-product, external service, and data model dependencies
5. **Risk validation** — validate and expand the risk register from the charter
6. **Resource confirmation** — confirm resource allocation with COO and CRO

**Output:** Sprint Zero assessment

---

### Phase 4: Sprint Planning
**Who triggers:** Engineering, after Sprint Zero

1. **Estimate** — For each story, estimate effort (tokens, compute, wall-clock time) and identify the engineering specialty needed
2. **Identify Dependencies** — Map cross-product dependencies, external service dependencies, and data model prerequisites
3. **Risk Assessment** — Flag technical risks and propose mitigations
4. **Pod Assignment** — Assign stories to pods based on specialty and capacity
5. **Resource Request** — Submit resource requirements to COO → CRO for allocation
6. **Execution Plan** — Produce sprint execution plan with story assignments, sub-agent spawn schedule, integration points, and demo readiness criteria

**Output:** Sprint execution plan

**Review:** COO reviews for resource feasibility. CoS confirms resource allocation.

**PLM role:** PLM is briefed on the sprint execution plan and begins active project monitoring.

---

### Phase 5: Sprint Execution
**Who triggers:** Engineering, continuously

- Engineering spawns sub-agents to execute stories
- Engineering tracks story completion against the sprint execution plan daily
- Engineering monitors sub-agent resource consumption in real-time against allocation
- **Daily standups** (async) via status updates
- **PLM actively monitors** cross-functional blockers — flags to relevant domain leads, escalates to CoS if unresolvable
- **COO monitors** operational blockers (infrastructure, vendor, deployment)
- **CoS receives escalations** from PLM (or CSO for strategic projects); does NOT monitor directly

---

### Phase 6: Sprint Review
**Who triggers:** Engineering, at sprint end

**Participants:** Engineering, PLM, CSO, COO, CoS, and any agents who contributed (Production, Logistics, CXO as needed)

**Agenda:**
1. **Engineering demo** — demonstrate completed stories against acceptance criteria
2. **PLM validation** — validate product correctness and roadmap alignment
3. **CXO review** — validate UX consistency and documentation accuracy (if UX changes)
4. **Production readiness** — Production confirms deployment readiness (if applicable)
5. **CSO strategic check** — assess strategic alignment
6. **Retrospective** — what went well, what didn't, lessons learned
7. **Risk register update** — review and update risks for next sprint
8. **Release approval** — PLM confirms the release is ready to proceed
9. **Go/No-Go decision** — All participants decide: continue, cancel, or rebaseline

**Output:** Sprint report

---

### Phase 7: Release
**Who triggers:** Production, after PLM release approval from sprint review

**Release Readiness Review** (convened by Production, participants: GC, CXO, CMO, CoS, PLM/CSO):
- GC confirms: all new component licenses are compatible
- CXO confirms: user/installation documentation is ready to update
- CMO confirms: go-to-market materials are ready
- PLM confirms: product is validated and ready for production
- CoS confirms: no cross-domain blockers remain

**Deployment sequence:**
1. Production deployment
2. CXO documentation update
3. CMO notification for go-to-market updates
4. CoS notification that release is complete
5. CSO notification for portfolio tracking

---

### Phase 8: Project Complete → Roadmap Update
**Who triggers:** PLM (or CSO for strategic projects), after the final sprint review

**Completion criteria (ALL must be satisfied):**
- All charter objectives met and verified against success criteria
- All stories meet the Standard Definition of Done
- Release is complete and deployed to production
- GC licensing review is clean
- CXO documentation is up to date
- The monitoring agent has validated product correctness

---

### Phase 9: Project Close
**Who triggers:** CoS, after PLM completes Phase 8

1. **Lessons learned** — The monitoring agent produces a lessons learned document
2. **Resource release** — CoS notifies CRO to release all project resource allocations
3. **Stakeholder notification** — CoS notifies all stakeholders that the project is formally closed
4. **Archive** — All project artifacts are archived

---

## Standard Definition of Done

Every story must meet these criteria before it is considered complete:

- [ ] Code implemented and reviewed
- [ ] Tests written and passing
- [ ] Security scan clean (no critical/high vulnerabilities)
- [ ] Documentation updated (code comments, README, API docs as applicable)
- [ ] UX reviewed by CXO (if user-facing changes)
- [ ] Licensing reviewed by GC (if new third-party components)
- [ ] Deployed to staging and verified by Production
- [ ] Acceptance criteria met and verified by PLM
- [ ] No unresolved critical or high-severity bugs

---

## Other Triggers

### Trigger: New Product Line
**Who triggers:** CSO with CEO approval

**CSO runs:**
1. `plm-instantiate` — creates a new PLM instance for the product line
2. New PLM receives the product concept brief as their starting specification
3. New PLM owns the new product line repo and manages token delegation

**CoS notifies COO to spawn (for the new product line):**
- **Engineering** (always)
- **Logistics** (if needed)
- **Production** (if needed)

---

## Agent Ownership

### C-Suite (direct reports to CoS)

| Agent | Owned By | Triggered By |
|---|---|---|
| **CSO** | Corporate strategic direction, compliance, M&A evaluation | Charter approval (with COO); CEO direction |
| **CFO** | Financial planning, budgeting, audit readiness, financial reporting | Weekly finance sync; CoS coordination |
| **COO** | Operations governance, vendor management, cross-domain resource allocation | Charter approval (with CSO) |
| **CMO** | Corporate and product marketing strategy, brand, campaigns | CEO direction (via CoS); PLM competitive research input |
| **CXO** | Cross-product user experience, design systems, documentation generation | CEO direction (via CoS); periodic schedule |
| **GC** | Legal review, contracts, compliance, IP, regulatory, licensing | CEO direction (via CoS) |

### Domain Leads (report through C-Suite)

| Agent | Owns | Reports To | Triggered By |
|---|---|---|---|
| **PLM** | Product roadmap, product specification, competitive analysis; owns product line repo | CSO | Engineering project complete; CEO direction |
| **CRO** | Resource aggregation, software licensing, tooling | CoS | Weekly aggregation cycle; CoS limit-setting |
| **Engineering** | Sprint planning, code execution, technical architecture, sub-agent management | COO | COO after charter approval |
| **Production** | Deployment pipelines, environment management, release coordination | COO | COO after charter approval |
| **Logistics** | Supply chain, procurement logistics, vendor coordination | COO | COO after charter approval |

### Executive Assistants *(outside agent reporting hierarchy)*

| EA | Serves | Triggered By |
|---|---|---|
| **[Name]** | [Human Leader] | Human direction |

---

## Key Rules

- **Main (OWL) only spawns CoS.** CoS spawns and manages all domain agents.
- **Executive Assistants are outside the agent reporting structure.** Each EA reports directly to their human counterpart, not to CoS.
- **CoS does not touch documents owned by others.** Each agent owns their own artifacts.
- **CoS orchestrates, not executes.** CoS coordinates, routes, tracks, and escalates. Domain agents do the work.
- **PLM does not touch sprint plans** (Engineering owned).
- **Engineering does not touch the roadmap or spec** (PLM owned).
- **Documents are only updated by their owners.**
- **Every story must meet the Standard Definition of Done** before it is considered complete.
