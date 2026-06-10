# Project Charter Template

*A project is a discrete body of work to build, extend, or improve a product. This charter defines the project's scope, objectives, resources, and success criteria. The project ends when the charter objectives are met or the project is cancelled.*

*Completed by: CSO, PLM, COO*
*Reviewed by: CFO, GC*
*Approved by: CSO, COO, CFO (+ CEO if cash required)*

---

## Project Identification

| Field | Detail |
|---|---|
| **Project Name** | |
| **Project ID** | |
| **Date Drafted** | |
| **Charter Version** | |
| **Prepared By** | CSO: / PLM: / COO: |
| **Initiated By** | CSO (new product) / PLM (capability gap) |
| **CEO Pre-Approval** | Required for new product creation |

---

## 1. Project Purpose

**Problem Statement:**
> What problem does this project solve? Why does it matter now?

**Strategic Alignment:**
> How does this project advance the product's competitive position? Which roadmap epic(s) does it address?

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

### 4.1 Engineering

| Entity | Role | Estimated Allocation |
|---|---|---|
| Engineering | Implementation Lead | __% of capacity |
| Sub-agents | As needed per pod model | As needed |

### 4.2 Other Entities

| Entity Needed? (Y/N) | Entity | Role | Justification |
|---|---|---|---|
| | Logistics | | |
| | Production | | |

### 4.3 Infrastructure / Compute

| Resource | Estimate | Duration |
|---|---|---|
| CI/CD compute | | |
| Cloud infrastructure | | |
| Third-party services | | |

### 4.4 Third-Party Components / Licensing

| Component | License Type | Compatible with commercial use? (Y/N) | GC Review Required? |
|---|---|---|---|
| | | | |

### 4.5 Cash Required?

| Item | Amount | One-time or Recurring | Approval Required From |
|---|---|---|---|
| | $ | → CEO approval needed | CEO |

**☐ No cash required** → CEO approval not needed
**☐ Cash required** → CEO approval required

---

## 5. Timeline & Milestones

| Milestone | Target Date | Owner |
|---|---|---|
| Charter Approved | — | CoS circulates for approval |
| Sprint Zero Complete | — | Engineering |
| Engineering Plan Confirmed | — | Engineering |
| Sprint Reviews | Per cadence | Engineering |
| Release | — | Production |
| Project Complete | — | Engineering |

*Engineering confirms dates after Sprint Zero and before execution begins.*

---

## 6. Dependencies & Risks

| Risk / Dependency | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| | | | | |

*Risk register is a living document — updated at each sprint review and when risks materialize.*

---

## 7. Definition of Done

*All stories in this project must meet the standard DoD. Project-specific additions:*

- [ ] *(Add project-specific DoD criteria here)*

---

## 8. Approvals

| Role | Name | Signature / Approval | Date |
|---|---|---|---|
| **CSO** (co-author + approve) | | | |
| **COO** (co-author + approve) | | | |
| **CFO** (review + approve) | | | |
| **GC** (review licensing) | | | |
| **CEO** (approve if cash required) | | | |

---

## 9. Execution Notes

*For Engineering — to be completed after charter approval:*

| Field | Detail |
|---|---|
| Sprint Zero Assessment Location | `docs/sprint-zero-assessment.md` |
| Sprint Plan Location | `docs/sprint-N-execution-plan.md` |
| Sprint Report Location | `docs/sprint-N-report.md` |
| Branch Prefix | `feature/{product}-{story-id}-` |
| Target Branch | `develop` |

---

*This charter authorizes the COO to spawn Engineering (and Logistics/Production if checked above). PLM monitors execution and escalates cross-domain issues to CoS. On project completion, PLM spawns automatically to refresh the roadmap and update the product specification.*
