# Project Charter Template

*A project is a discrete body of work to build, extend, or improve a product. This charter defines the project's scope, objectives, resources, and success criteria. The project ends when the charter objectives are met or the project is cancelled.*

## Charter Scope Rules

**A project charter authorizes a PROJECT — not a sprint, not an epic, not a feature.**

- **Project:** A discrete product or major product line that requires CEO approval, multi-sprint execution, and has its own success criteria and budget. Examples: "LifeBlood Through MVP," "LifeBlood Mobile Companion App."
- **Sprint:** A time-boxed execution phase within a project. Sprint approval is implied by the project charter. No separate charter needed.
- **Epic/Feature:** A body of work within a project roadmap. Tracked via backlog and roadmap, not via charter.
- **Work Package:** A feature area, story group, or task cluster within a sprint. Managed by Engineering via task decomposition, not via charter.

**One product = one active project charter.** The charter authorizes the COO to delegate all work within the project scope to Engineering. Sprints execute against the charter. Epics and features are backlog items within the charter's scope.

**New charters are needed only when:**
- A new product line is initiated (not a feature within an existing product)
- A materially different business model or market segment requires separate authorization
- Cash commitments exceed what the active charter authorizes
- EVM metrics on project execution require intervention (SPI/CPI < 0.9)

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
