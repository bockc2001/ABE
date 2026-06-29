# Project Charter Template

*A project is a discrete body of work to build, extend, or improve a product. This charter defines the project's scope, objectives, resources, and success criteria. The project ends when the charter objectives are met or the project is cancelled.*

## Charter Scope Rules

**A project charter authorizes a PROJECT — the highest-level unit of work.**

### Work Hierarchy

| Level | Description | Timescale | Our Equivalent |
|---|---|---|---|
| **Project** | Highest-level unit. Specific business objective, constrained by scope, schedule, budget, resources. Requires team-scale effort. | 3-5 years | LifeBlood Through MVP (LB-MVP-001) |
| **Program Increment (PI)** | Collection of sprints over which features and epics are planned. | ~3 months | Milestone groupings (M1-M4, M5-M8, etc.) |
| **Epic** | Major capability or workstream within a project. Too large for one sprint, broken into features/stories/tasks. | Multiple sprints | Epic 1 (HCM), Epic 5 (Visibility), Epic 6 (Scale), etc. |
| **Sprint** | Fixed-length execution period. Team commits to a specific set of work. | 1-2 weeks | Sprint 1...14 |
| **Feature/Story/Task** | Work items decomposed for sprint execution. | 1-3 days | GitHub issues |

### What Needs a Charter

- **Project** → **Always.** CEO approval required. Defines scope, budget, schedule, resources.
- **Program Increment** → **Sometimes.** If the PI introduces major scope changes, new dependencies, or requires cash beyond the project charter's authorization.
- **Epic** → **Never.** Managed via backlog and roadmap within the project.
- **Sprint** → **Never.** Sprint approval is implied by the project charter. COO delegates sprint scope to Engineering within the charter resource envelope.

**One product = one active project charter.** The charter authorizes PIs, sprints, epics, and features within its scope. New project charters are needed only for new product lines or when business objectives fundamentally change.

### EVM Escalation

If project EVM metrics breach thresholds (SPI/CPI < 0.9), the COO escalates to CoS for charter-level review.

**Workflow:** PLM proposes → COO estimates effort → CFO confirms resources → GC identifies legal issues → CSO confirms strategic alignment → PLM iterates → CEO final approval.

**Initiated by:** PLM (capability gap, competitive finding, design thinking output) or CSO (new product)

---

## Project Identification

| Field | Detail |
|---|---|
| **Project Name** | |
| **Project ID** | |
| **Date Drafted** | |
| **Charter Version** | |
| **Proposed By** | PLM: |
| **Initiated By** | PLM (capability gap / competitive finding / design thinking) / CSO (new product) |

---

## 1. Project Purpose

**Problem Statement:**
> What problem does this project solve? Why does it matter now?

**Strategic Alignment:**
> How does this project advance the product's competitive position? Which roadmap epic(s) does it addresses?

**Source:**
> What triggered this project? (competitive gap, customer need, design thinking output, technical debt, CSO strategic initiative)

---

## 2. Scope

**In Scope:**
> Specific features, stories, or deliverables included.

**Out of Scope:**
> Explicitly excluded items to prevent scope creep.

**Roadmap References:**
> Epic IDs and story IDs from the product roadmap:

| Epic ID | Story IDs | Priority |
|---|---|---|
| | | |

---

## 3. Objectives & Success Criteria

| Objective | Measurable Target | How Verified |
|---|---|---|
| | | |

---

## 4. Resource Requirements

### 4.1 Engineering Effort Estimate *(completed by COO)*

| Entity | Role | Estimated Allocation | Estimated Duration |
|---|---|---|---|
| Engineering | Implementation Lead | __% of capacity | __ sprints |
| Pods (ephemeral) | Execution | As needed per task decomposition | — |

**COO Effort Assessment:**
> Is this effort feasible given current operational capacity? What are the risks?

| Field | Detail |
|---|---|
| **Feasibility** | Feasible / Feasible with risks / Not feasible |
| **Risks** | |
| **COO Signature** | |

### 4.2 Other Entities

| Entity Needed? (Y/N) | Entity | Role | Justification |
|---|---|---|---|
| | Production | | |
| | Logistics | | |

### 4.3 Infrastructure / Compute

| Resource | Estimate | Duration |
|---|---|---|
| CI/CD compute | | |
| Cloud infrastructure | | |
| Third-party services | | |

### 4.4 Third-Party Components / Licensing *(completed by GC)*

| Component | License Type | Compatible with commercial use? (Y/N) | GC Review Required? |
|---|---|---|---|
| | | | |

**GC Legal Review:**
> Are there any legal, licensing, IP, or regulatory concerns with this project? Any third-party terms-of-service issues? Data handling or compliance risks?

| Field | Detail |
|---|---|
| **Legal Issues Identified** | None / List below |
| **Details** | |
| **GC Signature** | |

### 4.5 Cash & Resource Confirmation *(completed by CFO)*

| Item | Amount | One-time or Recurring |
|---|---|---|
| | $ | |

**CFO Resource Confirmation:**
> Are there sufficient resources (token budget, compute, tooling, cash) to complete this project? What is the estimated total cost?

| Field | Detail |
|---|---|
| **Sufficient Resources?** | Yes / No / Conditional |
| **Estimated Total Cost** | |
| **Budget Impact** | |
| **CFO Signature** | |

**☐ No cash required** → CEO approval needed for scope only
**☐ Cash required** → CEO approval required for budget release

---

## 5. Timeline & Milestones

| Milestone | Target Date | Owner |
|---|---|---|
| Charter Proposed by PLM | — | PLM |
| COO Effort Estimate Complete | +1 day | COO |
| CFO Resource Confirmation | +1 day | CFO |
| GC Legal Review Complete | +1 day | GC |
| CSO Strategic Review Complete | +1 day | CSO |
| PLM Iteration (if needed) | As needed | PLM |
| CEO Final Approval | After all reviews | CEO |
| Sprint Zero Complete | Post-approval | Engineering |
| Sprint Execution | Per cadence | Engineering |
| Release | — | Production |
| Project Complete | — | Engineering |

---

## 6. Dependencies & Risks

| Risk / Dependency | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| | | | | |

*Risk register is a living document — updated at each sprint review and when risks materialize.*

---

## 7. Strategic Alignment *(completed by CSO)*

**CSO Strategic Review:**
> Does this project align with the company's strategic goals? Does it advance competitive positioning? Are there strategic risks or opportunities?

| Field | Detail |
|---|---|
| **Strategic Alignment** | Aligned / Aligned with concerns / Not aligned |
| **Competitive Impact** | |
| **Strategic Risks** | |
| **CSO Signature** | |

---

## 8. Definition of Done

*All stories in this project must meet the standard DoD. Project-specific additions:*

- [ ] *(Add project-specific DoD criteria here)*

---

## 9. Approvals

| Role | Name | Responsibility | Signature / Approval | Date |
|---|---|---|---|---|
| **PLM** | | Propose charter, iterate until all reviews pass | | |
| **COO** | | Estimate effort, assess feasibility | | |
| **CFO** | | Confirm sufficient resources | | |
| **GC** | | Identify legal issues | | |
| **CSO** | | Confirm strategic alignment | | |
| **CEO** | | Final approval | | |

---

## 10. Execution Notes

*For Engineering — to be completed after charter approval:*

| Field | Detail |
|---|---|
| Sprint Zero Assessment Location | `docs/sprint-zero-assessment.md` |
| Sprint Plan Location | `docs/sprint-N-execution-plan.md` |
| Sprint Report Location | `docs/sprint-reviews/sprint-N-report.md` |
| Branch Prefix | `feature/{product}-{story-id}-` |
| Target Branch | `develop` |

---

*This charter authorizes the COO to spawn Engineering (and Logistics/Production if checked above). PLM monitors execution and escalates cross-domain issues to CoS. On project completion, PLM refreshes the roadmap and updates the product specification.*
