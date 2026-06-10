# ABE Ontology

## Overview

This document defines the formal ontology of an Automated Business Entity (ABE) — the entities, relationships, attributes, and rules that constitute the ABE as a structured system.

---

## Entity Types

### 1. Entity (Abstract Base)

The root of the ontology. All things in the ABE are entities.

| Attribute | Type | Description |
|---|---|---|
| `id` | UUID | Unique identifier |
| `name` | String | Human-readable name |
| `status` | Enum: `active`, `inactive`, `archived` | Current state |
| `created_at` | Timestamp | When the entity was created |

---

### 2. Human

A real person participating in the ABE.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `role` | String | Title/function (e.g., "CEO", "Operations") |
| `email` | String | Contact address |

**Relationships:**
- `leads` → Organization (0..1)
- `counterpart_of` → Agent (0..1)
- `served_by` → ExecutiveAssistant (0..1)

---

### 3. Organization

The ABE instance itself — the business entity.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `domain` | String | Business domain / industry |
| `framework_version` | String | ABE framework version |

**Relationships:**
- `led_by` → Human (1) — the primary leader (CEO)
- `employs` → Human (*)
- `has_agents` → Agent (*)
- `has_product_lines` → ProductLine (*)
- `has_projects` → Project (*)

---

### 4. Agent

An AI role within the ABE. Not a persistent identity — an agent is a role that can be instantiated, activated, or deactivated.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `role_code` | String | Canonical role code (`COS`, `CFO`, `COO`, `CSO`, `CMO`, `CRO`, `CXO`, `GC`, `PLM`, `ENGINEERING`, `PRODUCTION`, `LOGISTICS`) |
| `persona_name` | String | Short American name (first letter matches role code) |
| `reporting_to` | Agent | Direct reporting target |

**Relationships:**
- `reports_to` → Agent (1) — direct manager
- `manages` → Agent (*) — direct reports
- `counterpart_of` → Human (0..1) — direct human counterpart
- `monitors` → Project (*) — projects this agent monitors |
- `executes` → Project (*) — projects this agent executes |
- `owns` → Document (*) — artifacts this agent owns |
- `contributes_to` → Sprint (*) — sprints this agent participates in |
- `rolls_up_to` → Human (1) — transitive closure through reporting chain |

**Constraints:**
- `COUNTERPART_AGENT` ∈ {COS, CSO} — only CoS and CSO have direct human counterparts
- `REPORTING_TREE` is acyclic
- Every agent except COUNTERPART_AGENTs `ROLLS_UP_TO` exactly one human via transitive `reports_to`

---

### 5. ExecutiveAssistant

An AI agent outside the reporting hierarchy, serving a human leader directly.

**Inherits from:** Entity (not Agent)

| Attribute | Type | Description |
|---|---|---|
| `serves` | Human | The human this EA supports |
| `persona_name` | String | Short American name (first letter matches human's name) |

**Relationships:**
- `serves` → Human (1)
- `supports_org` → Agent (*) — agents in the org this EA coordinates with |

**Constraints:**
- ExecutiveAssistant ∉ Agent reporting tree
- `serves` is exactly 1 human
- `persona_name` first letter == `served_human.name` first letter

---

### 6. ProductLine

A product owned and evolved by the ABE.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `repo_url` | URL | GitHub repo for this product line |
| `target_market` | String | Description of target customers |
| `status` | Enum: `concept`, `active`, `sunset`, `archived` | Lifecycle stage |

**Relationships:**
- `managed_by` → PLM (1) — the PLM responsible for this product line
- `has_projects` → Project (*) — projects within this product line
- `has_roadmap` → Roadmap (1) — the product roadmap
- `owned_by` → Organization (1)

---

### 7. Roadmap

The planned evolution of a product line.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `horizon_sprints` | Integer | How many sprints ahead are planned (typically 3-5) |

**Relationships:**
- `for_product_line` → ProductLine (1)
- `contains_epics` → Epic (*)
- `owned_by` → PLM (1)

---

### 8. Epic

A large body of work within a product line, spanning one or more sprints.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `priority` | Enum: `P0`, `P1`, `P2` | Priority level |
| `status` | Enum: `planned`, `in_progress`, `complete`, `cancelled` |

**Relationships:**
- `on_roadmap` → Roadmap (1)
- `contains_stories` → Story (*)
- `belongs_to` → ProductLine (1)

---

### 9. Project

A discrete body of work to build, extend, or improve a product.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `charter_url` | URL | Link to the project charter |
| `status` | Enum: `planning`, `active`, `review`, `closing`, `closed`, `cancelled` |
| `cash_required` | Boolean | Whether CEO approval is needed |
| `strategic` | Boolean | Whether CSO acts as PLM |

**Relationships:**
- `for_product_line` → ProductLine (1)
- `charter_approved_by` → Agent (*) — CSO, COO, CFO, GC, CEO
- `executed_by` → Engineering (1) — Engineering owns execution
- `monitored_by` → Agent (1) — PLM (or CSO for strategic projects)
- `has_sprints` → Sprint (*)
- `has_stories` → Story (*)
- `budget_allocated_by` → CRO (1)

**Constraints:**
- If `cash_required` = true, `charter_approved_by` must include CEO
- If `strategic` = true, `monitored_by` = CSO
- Project follows lifecycle: `planning → active → review → closing → closed`

---

### 10. Sprint

A time-boxed iteration within a project.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `sprint_number` | Integer | Sequential within project |
| `start_date` | Date |
| `end_date` | Date |
| `status` | Enum: `planning`, `zero`, `executing`, `review`, `complete` |

**Relationships:**
- `part_of` → Project (1)
- `contains_stories` → Story (*)
- `execution_plan_owned_by` → Engineering (1)
- `review_participants` → Agent (*) — Engineering, PLM, CSO, COO, CoS

---

### 11. Story

A single unit of work within a sprint.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `story_id` | String | Unique identifier |
| `title` | String |
| `description` | String |
| `priority` | Enum: `P0`, `P1`, `P2` |
| `size` | Enum: `S`, `M`, `L` | Estimated effort |
| `status` | Enum: `backlog`, `planned`, `in_progress`, `review`, `done`, `deferred` |
| `acceptance_criteria` | List[String] | Conditions for completion |

**Relationships:**
- `part_of` → Sprint (0..1)
- `part_of_epic` → Epic (0..1)
- `assigned_to` → Pod (0..1)
- `estimated_by` → Engineering (1)
- `validated_by` → PLM (1)

---

### 12. Pod

A team of sub-agents working on a set of stories within a sprint.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `specialty` | Enum: `SOFTWARE`, `SYSTEMS`, `MECHANICAL`, `ELECTRICAL` | Engineering specialty |

**Relationships:**
- `spawned_by` → Engineering (1)
- `works_on` → Story (*)
- `part_of` → Sprint (1)

---

### 13. Document

An artifact produced within the ABE.

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `doc_type` | Enum: `CHARTER`, `SPRINT_PLAN`, `SPRINT_REPORT`, `CSO_REVIEW`, `COO_READINESS`, `ROADMAP`, `SPEC`, `ADR`, `EVM_REPORT`, `LESSONS_LEARNED` |
| `location` | URL | File path or link |
| `version` | String | Semantic version |

**Relationships:**
- `owned_by` → Agent (1) — the agent who controls this document
- `reviewed_by` → Agent (*)
- `pertains_to` → Entity (0..1) — project, sprint, or epic this document describes |

**Constraints:**
- Documents are only updated by their `owned_by` agent
- Each document type has a canonical location convention

---

### 14. SubAgent

An ephemeral agent spawned by Engineering to execute a specific task.

**Inherits from:** Entity (not Agent)

| Attribute | Type | Description |
|---|---|---|
| `task_brief` | String | Description of the task |
| `specialty` | Enum: `SOFTWARE`, `SYSTEMS`, `MECHANICAL`, `ELECTRICAL` |
| `status` | Enum: `spawned`, `executing`, `review`, `integrated`, `closed` |

**Relationships:**
- `spawned_by` → Engineering (1)
- `consumes_allocation_from` → Engineering (1) — inherits resource limits
- `works_on` → Story (0..1)

**Constraints:**
- SubAgent lifecycle: `spawned → executing → review → integrated → closed`
- All resource consumption is debited from Engineering's allocation

---

## Relationship Summary

```
Organization
  ├── led_by → Human (CEO)
  ├── employs → Human (*)
  ├── has_agents → Agent (*)
  ├── has_product_lines → ProductLine (*)
  └── has_projects → Project (*)

Human
  ├── counterpart_of → Agent (0..1)
  └── served_by → ExecutiveAssistant (0..1)

Agent
  ├── reports_to → Agent (1)
  ├── manages → Agent (*)
  ├── counterpart_of → Human (0..1)
  ├── monitors → Project (*)
  ├── executes → Project (*)      [Engineering only]
  ├── owns → Document (*)
  └── rolls_up_to → Human (1)      [transitive]

ExecutiveAssistant
  └── serves → Human (1)

ProductLine
  ├── managed_by → PLM (1)
  ├── has_projects → Project (*)
  └── has_roadmap → Roadmap (1)

Roadmap
  └── contains_epics → Epic (*)

Epic
  └── contains_stories → Story (*)

Project
  ├── for_product_line → ProductLine (1)
  ├── executed_by → Engineering (1)
  ├── monitored_by → PLM | CSO (1)
  ├── has_sprints → Sprint (*)
  └── charter_approved_by → Agent (*)

Sprint
  ├── part_of → Project (1)
  ├── contains_stories → Story (*)
  └── contains_pods → Pod (*)

Story
  ├── part_of → Sprint (0..1)
  ├── part_of_epic → Epic (0..1)
  ├── assigned_to → Pod (0..1)
  └── validated_by → PLM (1)

Pod
  ├── spawned_by → Engineering (1)
  └── works_on → Story (*)

SubAgent
  ├── spawned_by → Engineering (1)
  └── consumes_allocation_from → Engineering (1)

Document
  ├── owned_by → Agent (1)
  └── pertains_to → Entity (0..1)
```

---

## Axioms (Invariants)

These rules always hold in a valid ABE state:

### Organizational Invariants (R1)
1. **CoS always exists** — every ABE has exactly one CoS
2. **CSO always exists** — every ABE has exactly one CSO
3. **Reporting tree is acyclic** — no circular reporting chains
4. **Every agent rolls up to the CEO** — through at most 3 hops
5. **Only CoS and CSO have direct human counterparts**

### EA Invariants (R2)
6. **EAs are outside the reporting tree** — no `reports_to` edge from any EA
7. **Each EA serves exactly one human**
8. **EA persona name matches human name initial**

### Product Invariants (R3)
9. **Each product line has exactly one PLM**
10. **Each product line has exactly one roadmap**
11. **Roadmap horizon is 3-5 sprints**

### Project Invariants (R4)
12. **Every project belongs to exactly one product line**
13. **Every project has exactly one charter**
14. **Engineering executes every project**
15. **PLM monitors every non-strategic project**
16. **CSO monitors every strategic project**
17. **Projects follow the lifecycle**: planning → active → review → closing → closed

### Sprint Invariants (R5)
18. **Sprints belong to exactly one project**
19. **Stories belong to at most one sprint**
20. **Sprint execution plan is owned by Engineering**
21. **Sprint review includes Engineering, PLM, CSO, COO, CoS**

### Document Invariants (R6)
22. **Every document has exactly one owner**
23. **Documents are only updated by their owner**
24. **Sprint artifacts live in the product line repo**

### Resource Invariants (R7)
25. **Every agent reports resource consumption to CRO**
26. **CoS sets hard limits at ≤80% of available**
27. **Sub-agent consumption is debited from Engineering's allocation**

---

## State Machines

### Project State Machine
```
  planning ──charter_approved──▶ active ──all_sprints_complete──▶ review
     │                              │                               │
     │                              │                               │
   cancelled                   cancelled                       closing
                                                                   │
                                                                   │
                                                                  closed
```

### Sprint State Machine
```
  planning ──sprint_zero──▶ zero ──execution_begins──▶ executing ──review_complete──▶ complete
                                                                                       │
                                                                                       │
                                                                                    (next sprint)
```

### Story State Machine
```
  backlog ──planned──▶ planned ──assigned──▶ in_progress ──submitted──▶ review ──▶ done
                                                                                │
                                                                             deferred
```

### Sub-Agent State Machine
```
  spawned ──begins_work──▶ executing ──output_ready──▶ review ──approved──▶ integrated ──▶ closed
                                                          │
                                                       rejected ──▶ executing
```

---

## File Locations

ABE documents follow a canonical location convention:

| Document Type | Location Pattern |
|---|---|
| Project charter | `<product-repo>/docs/charters/<project-id>.md` |
| Sprint execution plan | `<product-repo>/docs/sprint-N-execution-plan.md` |
| Sprint report | `<product-repo>/docs/sprint-N-report.md` |
| CSO sprint review | `<product-repo>/docs/sprint-reviews/cso/sprintN-cso-review.md` |
| COO operational readiness | `<product-repo>/docs/sprint-reviews/coo/sprintN-operational-readiness.md` |
| Product roadmap | `<product-repo>/docs/product-roadmap.md` |
| Product spec | `<product-repo>/docs/product-spec.md` |
| Architecture decision record | `<product-repo>/docs/adr/ADR-NN-<title>.md` |
| EVM report | `<product-repo>/docs/evm/sprintN-evm-report.md` |
| Lessons learned | `<product-repo>/docs/lessons-learned/<project-id>.md` |
| PLM workflow | `<instance>/workflows/plm/<product>/workflow.md` |
| Instance agent roster | `<instance>/docs/agent-roster.md` |
| PLM instances registry | `<instance>/plm-instances.json` |
| Human counterpart bindings | `<instance>/human-counterpart-bindings.json` |
