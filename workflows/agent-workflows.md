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

A **project** is a discrete body of work to build, extend, or improve a product. It is bounded by a charter (start) and a close (end), executed in sprints, and owned by Engineering with PLM monitoring. A product line is made up of successive projects.

---

### Phase 1: Project Charter Proposal
**Who proposes:** PLM (for capability gaps, competitive findings, design thinking outputs) or CSO (for new products)

**PLM initiates** when:
- Competitive analysis reveals a gap that requires a project to close
- Customer intelligence identifies a need that crosses multiple features
- Design thinking produces a feature concept significant enough for its own project
- The backlog contains items that cluster into a coherent project

**CSO initiates** when a new product line or major strategic initiative is identified (with CEO pre-approval).

**PLM drafts the charter** using the template at `abe/docs/charters/project-charter-template.md`:
1. Define project purpose, scope, objectives, success criteria
2. Identify risks and dependencies
3. Propose timeline and milestones
4. Document the source (competitive gap, customer need, design thinking output, etc.)

**Output:** Draft project charter (PLM authored)

---

### Phase 2: Charter Review & Iteration
**Who triggers:** PLM submits draft charter to reviewers

**Review sequence (PLM iterates between reviews until all pass):**

1. **COO** — Estimates engineering effort and duration. Assesses operational feasibility. Signs off on effort estimate.
2. **CFO** — Confirms sufficient resources exist (token budget, compute, tooling, cash). Estimates total cost. Signs off on resource confirmation.
3. **GC** — Identifies any legal, licensing, IP, regulatory, or compliance issues. Signs off on legal review.
4. **CSO** — Confirms strategic alignment. Assesses competitive impact and strategic risks. Signs off on strategic review.

**PLM iterates:** If any reviewer raises concerns, PLM revises the charter and resubmits to that reviewer. This continues until all four reviewers sign off.

**CEO Final Approval:** Once all reviewers approve, the charter goes to CEO for final sign-off.

**The charter IS the delegation authority.** On CEO signature:
1. **CoS immediately notifies COO** — the charter authorizes COO to act
2. **COO spawns Engineering** (and Logistics/Production if needed) — COO defines scope of work per the charter
3. **COO delegates sprint planning to Engineering** — Engineering owns task decomposition
4. **COO monitors execution** — tracks progress, reviews task decomposition, monitors resources

The charter's resource section (Section 4) is the COO's  delegation envelope — what spends are authorized, what entities are engaged, and the effort estimate COO uses to size the work.

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

> **Sprints do not need their own charter.** Sprint approval is implied by the project charter. The COO delegates sprint scope to Engineering within the charter's resource envelope. Only if EVM metrics on project execution require intervention (SPI/CPI thresholds breached) does the COO escalate to CoS for charter-level review.
>
> **Charter scope hierarchy:**
> - **Project charter** — authorizes a discrete product or major product line. One product = one active charter. Examples: "LifeBlood Through MVP."
> - **Sprint** — execution phase within a project. Implied by project charter. No separate charter.
> - **Epic/Feature** — body of work within the project roadmap. Backlog items, not charters.
> - **Work Package** — feature area or story cluster within a sprint. Engineering manages via task decomposition, not charter.
>
> **New charters only when:** new product line, different business model/market, cash beyond charter authorization, or EVM intervention required.

**Prerequisite:** Engineering has analyzed deltas and produced UML/SysML models representing the new features. PLM and CSO have reviewed those models.

1. **Estimate** — For each story, estimate effort (tokens, compute, wall-clock time) and identify the engineering specialty needed
2. **Identify Dependencies** — Map cross-product dependencies, external service dependencies, and data model prerequisites
3. **Risk Assessment** — Flag technical risks and propose mitigations
4. **Decompose features → tasks** — Each task is explicit, small, atomic, testable, and self-describing. Sized for one pod.
5. **Pod Assignment** — Assign tasks to pods based on specialty and capacity
6. **Resource Request** — Submit resource requirements to COO → CRO for allocation
7. **Execution Plan** — Produce sprint execution plan with task assignments, feature branch structure, and demo readiness criteria

**Output:** Sprint execution plan + task decomposition + branch structure

**Review:** COO reviews for resource feasibility. CoS confirms resource allocation.

**PLM role:** PLM is briefed on the sprint execution plan and begins active project monitoring.

---

### Phase 5: Sprint Execution
**Who triggers:** Engineering, continuously

**Branch & Integration Model:**
```
main (protected)
  └── develop (protected)
        └── feature/<feature-id>-<short-name>     ← Engineering creates
              ├── task/<feature-id>/<task-id>-<short-name>   ← Pod creates
              ├── task/<feature-id>/<task-id>-<short-name>
              └── task/<feature-id>/<task-id>-<short-name>
```

**Rules:**
1. Engineering creates one feature branch per feature from `develop`
2. Each pod creates a task sub-branch from the feature branch
3. Pods work only in their task branch — no modifications outside task scope
4. Pods PR **only to the parent feature branch** — never directly to `develop` or `main`
5. Pods may only PR when **all tests run clean** (unit tests, typecheck, lint)
6. Engineering reviews each pod PR before merging into the feature branch
7. When all tasks for a feature are complete, Engineering runs full test suite and PRs the feature branch to `develop`

**Execution:**
- Engineering spawns pods to execute tasks in their task branches
- Engineering tracks task completion against the sprint execution plan daily
- Engineering monitors sub-agent resource consumption in real-time against allocation
- Engineering reviews and merges pod PRs into feature branches
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

## Engineering Principles

All engineering work (Engineering, pods, sub-agents) MUST follow the **Engineering Principles** defined at `abe/workflows/coo/engineering/principles.md`. Summary:

| # | Principle | Check at PR |
|---|---|---|
| 1 | Think Before Coding | Model exists before code? |
| 2 | Simplicity First | Is there a simpler approach? |
| 3 | Surgical Changes | PR focused? No drive-by refactors? |
| 4 | Goal-Driven Execution | Task traces to a feature/goal? |
| 5 | Update Models Before Code | UML/SysML approved? |
| 6 | Technical Debt Accounted For | Discovered debt logged as backlog item? |
| 7 | Tests Represent the User | Tests cover real user workflows? |
| 8 | Trivial Tests Provide No Value | Any tests that can't catch real bugs? |
| 9 | Challenge → Clarify → Commit | Any unclarified assumptions in the code? |

Engineering enforces these during PR review. CoS reports compliance status to CEO.

## Standard Definition of Done

Every story must meet these criteria before it is considered complete:

- [ ] All applicable **Engineering Principles** satisfied
- [ ] UML/SysML models updated to represent the feature (class diagrams, requirement allocations, activity diagrams)
- [ ] Feature branch created from `develop` with all task sub-branches merged
- [ ] Code implemented and reviewed
- [ ] Tests written and passing (unit, typecheck, lint — all clean)
- [ ] Tests represent real user workflows (no trivial tests)
- [ ] Technical debt discovered during execution logged as backlog items
- [ ] Security scan clean (no critical/high vulnerabilities)
- [ ] Documentation updated (code comments, README, API docs as applicable)
- [ ] UX reviewed by CXO (if user-facing changes)
- [ ] Licensing reviewed by GC (if new third-party components)
- [ ] Deployed to staging and verified by Production
- [ ] Acceptance criteria met and verified by PLM
- [ ] No unresolved critical or high-severity bugs
- [ ] Full test suite passes on feature branch before PR to `develop`

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

## Main Does Not Build

**Main (OWL) is Chris's personal assistant and the orchestrator. It routes requests, answers questions, and tracks progress. It does NOT write product code, create branches, run builds, or deploy.**

When Chris requests a product change:
1. **Main describes the delta** — what exists, what should exist, and why
2. **Main routes to Engineering** through the hierarchy (Main → CoS → COO → Engineering)
3. **Main tracks** that the request was routed and monitors for completion
4. **Main does NOT implement** the change itself

If Main accidentally starts writing product code, stop. Route it to Engineering.

---

## Key Rules

- **Main (OWL) only spawns CoS.** CoS spawns and manages all domain agents.
- **Main does not build.** Main routes, tracks, and explains. Engineering builds.
- **Executive Assistants are outside the agent reporting structure.** Each EA reports directly to their human counterpart, not to CoS.
- **CoS does not touch documents owned by others.** Each agent owns their own artifacts.
- **CoS orchestrates, not executes.** CoS coordinates, routes, tracks, and escalates. Domain agents do the work.
- **PLM does not touch sprint plans** (Engineering owned).
- **Engineering does not touch the roadmap or spec** (PLM owned).
- **Documents are only updated by their owners.**
- **Every story must meet the Standard Definition of Done** before it is considered complete.
