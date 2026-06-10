# ABE Ontology

## Overview

This document defines the formal ontology of an Automated Business Entity (ABE) — the entities, relationships, attributes, artifacts, and rules that constitute the ABE as a structured system.

---

## Entity Hierarchy

```
Entity (abstract)
├── Human
├── Organization
├── Agent
├── ExecutiveAssistant
├── ProductLine
├── Roadmap
├── Epic
├── Project
├── Sprint
├── Story
├── Pod
├── SubAgent
└── Artifact
    ├── Document
    │   ├── Charter
    │   ├── SprintPlan
    │   ├── SprintReport
    │   ├── CSOReview
    │   ├── COOReadiness
    │   ├── RoadmapDoc
    │   ├── ProductSpec
    │   ├── ArchitectureDecision
    │   ├── EVMReport
    │   ├── CompetitiveAnalysis
    │   ├── GapAnalysis
    │   ├── SpikeReport
    │   ├── TestCoverageReport
    │   ├── PersonaDoc
    │   ├── UXAnalysis
    │   ├── UserManual
    │   ├── APIReference
    │   ├── InstallationGuide
    │   ├── DeveloperOnboarding
    │   ├── LessonsLearned
    │   ├── EngagementPlan
    │   ├── GTMReadiness
    │   ├── StakeholderUpdate
    │   └── ActualVsEstimated
    ├── Template
    │   ├── ProjectCharterTemplate
    │   ├── PLMTemplate
    │   ├── AgentRosterTemplate
    │   └── IssueTemplate
    ├── Workflow
    │   ├── AgentWorkflow
    │   ├── PLMWorkflow
    │   ├── ProjectLifecycleWorkflow
    │   └── ReportingProtocol
    ├── Skill
    │   ├── PLMInstantiate
    │   └── HumanCounterpart
    ├── Config
    │   ├── PLMInstances
    │   ├── HumanCounterpartBindings
    │   ├── BranchBacklog
    │   └── ReportingStructure
    └── FrameworkArtifact
        ├── ABEReadme
        ├── InstanceReadme
        ├── AgentWorkflowsIndex
        └── Ontology
```

---

## Core Entities

### 1. Entity (Abstract Base)

| Attribute | Type | Description |
|---|---|---|
| `id` | UUID | Unique identifier |
| `name` | String | Human-readable name |
| `status` | Enum | Current state (type-specific) |
| `created_at` | Timestamp | Creation time |
| `updated_at` | Timestamp | Last modification |

---

### 2. Human

A real person participating in the ABE.

| Attribute | Type | Description |
|---|---|---|
| `role` | String | Title/function |
| `email` | String | Contact address |

**Relationships:**
- `leads` → Organization (0..1)
- `counterpart_of` → Agent (0..1)
- `served_by` → ExecutiveAssistant (0..1)

---

### 3. Organization

The ABE instance itself.

| Attribute | Type | Description |
|---|---|---|
| `domain` | String | Business domain |
| `framework_version` | String | ABE framework version |

**Relationships:**
- `led_by` → Human (1)
- `employs` → Human (*)
- `has_agents` → Agent (*)
- `has_product_lines` → ProductLine (*)
- `has_projects` → Project (*)

---

### 4. Agent

An AI role within the ABE.

| Attribute | Type | Description |
|---|---|---|
| `role_code` | String | Canonical code: `COS`, `CFO`, `COO`, `CSO`, `CMO`, `CRO`, `CXO`, `GC`, `PLM`, `ENGINEERING`, `PRODUCTION`, `LOGISTICS` |
| `persona_name` | String | Named persona |
| `reporting_to` | Agent | Direct manager |

**Relationships:**
- `reports_to` → Agent (1)
- `manages` → Agent (*)
- `counterpart_of` → Human (0..1)
- `monitors` → Project (*)
- `executes` → Project (*)
- `owns` → Artifact (*)
- `contributes_to` → Sprint (*)
- `rolls_up_to` → Human (1) — transitive
- `spawns` → SubAgent (*) — Engineering only

**Constraints:**
- Only `COS` and `CSO` have direct human counterparts
- Reporting tree is acyclic
- Every agent `rolls_up_to` exactly one human

---

### 5. ExecutiveAssistant

An AI agent outside the reporting hierarchy.

| Attribute | Type | Description |
|---|---|---|
| `serves` | Human | The human this EA supports |
| `persona_name` | String | Matches human name initial |

**Constraints:**
- EA ∉ Agent reporting tree
- Each EA serves exactly 1 human

---

### 6. ProductLine

A product owned by the ABE.

| Attribute | Type | Description |
|---|---|---|
| `repo_url` | URL | GitHub repo |
| `target_market` | String | Target customers |
| `status` | Enum: `concept`, `active`, `sunset`, `archived` |

**Relationships:**
- `managed_by` → PLM (1)
- `has_projects` → Project (*)
- `has_roadmap` → Roadmap (1)
- `owned_by` → Organization (1)

---

### 7. Roadmap

Planned evolution of a product line.

| Attribute | Type | Description |
|---|---|---|
| `horizon_sprints` | Integer | Typically 3-5 |

**Relationships:**
- `for_product_line` → ProductLine (1)
- `contains_epics` → Epic (*)
- `owned_by` → PLM (1)

---

### 8. Epic

Multi-sprint body of work.

| Attribute | Type | Description |
|---|---|---|
| `priority` | Enum: `P0`, `P1`, `P2` |
| `status` | Enum: `planned`, `in_progress`, `complete`, `cancelled` |

**Relationships:**
- `on_roadmap` → Roadmap (1)
- `contains_stories` → Story (*)
- `belongs_to` → ProductLine (1)

---

### 9. Project

A discrete body of work bounded by a charter (start) and close (end). A project's **scope** is a subset of the product roadmap — a selection of epics (and their stories) that the project will deliver.

| Attribute | Type | Description |
|---|---|---|
| `charter_url` | URL | Link to charter artifact |
| `status` | Enum: `planning`, `active`, `review`, `closing`, `closed`, `cancelled` |
| `cash_required` | Boolean |
| `strategic` | Boolean |

**Relationships:**
- `for_product_line` → ProductLine (1)
- `charter_approved_by` → Agent (*)
- `executed_by` → Engineering (1)
- `monitored_by` → PLM | CSO (1)
- `has_sprints` → Sprint (*)
- `budget_allocated_by` → CRO (1)
- `has_charter` → Charter (1)
- `has_lessons` → LessonsLearned (0..1)
- `scope` → Epic (*) — the epics from the roadmap that define this project's scope
- `scope` → Story (*) — stories within those epics (transitive via Epic, or direct for cross-epic stories)
- `slice_of` → Roadmap (1) — the roadmap from which scope was selected

**Lifecycle:** `planning → active → review → closing → closed`

**Scope Selection:** During project planning (Phase 1), the PLM and CSO select a subset of roadmap epics to form the project's scope. The charter references these epics. As the roadmap evolves, the project scope is fixed at charter approval — roadmap changes do not retroactively alter an approved project scope (scope changes require charter amendment).

---

### 10. Sprint

Time-boxed iteration within a project.

| Attribute | Type | Description |
|---|---|---|
| `sprint_number` | Integer | Sequential |
| `start_date` | Date |
| `end_date` | Date |
| `status` | Enum: `planning`, `zero`, `executing`, `review`, `complete` |

**Relationships:**
- `part_of` → Project (1)
- `contains_stories` → Story (*)
- `contains_pods` → Pod (*)
- `has_plan` → SprintPlan (0..1)
- `has_report` → SprintReport (0..1)
- `has_cso_review` → CSOReview (0..1)
- `has_coo_readiness` → COOReadiness (0..1)
- `has_evm_report` → EVMReport (0..1)
- `has_actuals_report` → ActualVsEstimated (0..1)
- `execution_plan_owned_by` → Engineering (1)

**Lifecycle:** `planning → zero → executing → review → complete`

---

### 11. Story

A single unit of work.

| Attribute | Type | Description |
|---|---|---|
| `story_id` | String | Unique ID |
| `title` | String |
| `description` | String |
| `priority` | Enum: `P0`, `P1`, `P2` |
| `size` | Enum: `S`, `M`, `L` |
| `status` | Enum: `backlog`, `planned`, `in_progress`, `review`, `done`, `deferred` |
| `acceptance_criteria` | List[String] |

**Relationships:**
- `part_of` → Sprint (0..1)
- `part_of_epic` → Epic (0..1)
- `assigned_to` → Pod (0..1)
- `estimated_by` → Engineering (1)
- `validated_by` → PLM (1)

**Lifecycle:** `backlog → planned → in_progress → review → done` (or `deferred`)

---

### 12. Pod

A team of sub-agents within a sprint.

| Attribute | Type | Description |
|---|---|---|
| `specialty` | Enum: `SOFTWARE`, `SYSTEMS`, `MECHANICAL`, `ELECTRICAL` |

**Relationships:**
- `spawned_by` → Engineering (1)
- `works_on` → Story (*)
- `part_of` → Sprint (1)

---

### 13. SubAgent

An ephemeral agent spawned by Engineering for a specific task.

| Attribute | Type | Description |
|---|---|---|
| `task_brief` | String |
| `specialty` | Enum: `SOFTWARE`, `SYSTEMS`, `MECHANICAL`, `ELECTRICAL` |
| `status` | Enum: `spawned`, `executing`, `review`, `integrated`, `closed` |

**Relationships:**
- `spawned_by` → Engineering (1)
- `consumes_allocation_from` → Engineering (1)
- `works_on` → Story (0..1)

**Lifecycle:** `spawned → executing → review → integrated → closed` (or `rejected → executing`)

---

## Artifact Types

All artifacts are entities created and owned by agents. Each artifact type has a canonical owner, location, and lifecycle.

### 14. Artifact (Abstract Base)

**Inherits from:** Entity

| Attribute | Type | Description |
|---|---|---|
| `artifact_type` | Enum | Type discriminator |
| `repo` | Enum: `ABE`, `INSTANCE`, `PRODUCT` | Which repo it lives in |
| `location` | String | File path |
| `owned_by` | Agent (1) | The owning agent |
| `reviewed_by` | Agent (*) | Reviewers (if any) |

**Constraint:** Artifacts are only updated by their `owned_by` agent.

---

### Document Artifacts

#### 14.1 Charter
- **Owner:** CSO (co-authored with PLM, COO)
- **Location:** `<product-repo>/docs/charters/<project-id>.md`
- **Reviewers:** CFO, GC, CEO (if cash required)
- **Produced by:** Project Phase 1 (Planning)
- **Lifecycle:** `draft → reviewing → approved → superseded`
- **Pertains to:** Project

#### 14.2 SprintPlan
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/sprint-N-execution-plan.md` or `sprint-N-plan.md`
- **Produced by:** Project Phase 5 (Sprint Planning)
- **Pertains to:** Sprint

#### 14.3 SprintReport
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/sprint-N-report.md` or `sprint-N-p0-completion-report.md`
- **Produced by:** Project Phase 6 (Sprint Review)
- **Pertains to:** Sprint

#### 14.4 CSOReview
- **Owner:** CSO
- **Location:** `<product-repo>/docs/sprint-reviews/cso/sprintN-cso-review.md`
- **Produced by:** Project Phase 2 (CSO strategic review of sprint plan)
- **Pertains to:** Sprint

#### 14.5 COOReadiness
- **Owner:** COO
- **Location:** `<product-repo>/docs/sprint-reviews/coo/sprintN-operational-readiness.md`
- **Produced by:** Project Phase 4 (Pre-sprint operational readiness)
- **Pertains to:** Sprint

#### 14.6 RoadmapDoc
- **Owner:** PLM
- **Location:** `<product-repo>/docs/product-roadmap.md`
- **Produced by:** Continuous (PLM updates after each sprint)
- **Pertains to:** Roadmap

#### 14.7 ProductSpec
- **Owner:** PLM
- **Location:** `<product-repo>/docs/product-spec.md` or `product-spec-competitive-costpoint.md`
- **Produced by:** PLM continuous refinement
- **Pertains to:** ProductLine

#### 14.8 ArchitectureDecision (ADR)
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/architecture-decisions/NNN-title.md`
- **Produced by:** Engineering (when making significant technical decisions)
- **Pertains to:** ProductLine

#### 14.9 EVMReport
- **Owner:** PLM (or CSO for strategic projects)
- **Location:** `<product-repo>/docs/sprint-N-evm-report.md` or embedded in sprint report
- **Produced by:** End of each sprint
- **Pertains to:** Sprint

#### 14.10 CompetitiveAnalysis
- **Owner:** PLM
- **Location:** `<product-repo>/docs/competitive-gap-analysis.md`
- **Produced by:** PLM (continuous)
- **Pertains to:** ProductLine

#### 14.11 GapAnalysis
- **Owner:** PLM
- **Location:** `<product-repo>/docs/epic-N-gap-analysis.md`
- **Produced by:** PLM (when initiating a new project)
- **Pertains to:** Epic

#### 14.12 SpikeReport
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/spike-N-title.md` (e.g., `spike-8.5-persistence.md`)
- **Produced by:** Engineering (Sprint Zero or investigation spikes)
- **Pertains to:** Sprint or Project

#### 14.13 TestCoverageReport
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/test-coverage-analysis.md`
- **Produced by:** Engineering (end of sprint or on demand)
- **Pertains to:** Sprint

#### 14.14 PersonaDoc
- **Owner:** PLM
- **Location:** `<product-repo>/docs/personas.md`
- **Produced by:** PLM (initial creation, updated as market research evolves)
- **Pertains to:** ProductLine

#### 14.15 UXAnalysis
- **Owner:** CXO
- **Location:** `<product-repo>/docs/ux-analysis.md`
- **Produced by:** CXO (periodic, every 3-5 sprints)
- **Pertains to:** ProductLine

#### 14.16 UserManual
- **Owner:** CXO
- **Location:** `<product-repo>/docs/user-manual.md`
- **Produced by:** CXO (updated each release)
- **Pertains to:** ProductLine

#### 14.17 APIReference
- **Owner:** CXO
- **Location:** `<product-repo>/docs/api-reference.md`
- **Produced by:** CXO (updated each release)
- **Pertains to:** ProductLine

#### 14.18 InstallationGuide
- **Owner:** CXO
- **Location:** `<product-repo>/docs/installation.md`
- **Produced by:** CXO (updated each release)
- **Pertains to:** ProductLine

#### 14.19 DeveloperOnboarding
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/developer-onboarding.md`
- **Produced by:** Engineering
- **Pertains to:** ProductLine

#### 14.20 LessonsLearned
- **Owner:** Engineering (or PLM for strategic projects)
- **Location:** `<product-repo>/docs/lessons-learned/<project-id>.md`
- **Produced by:** Project Phase 8 (Close)
- **Pertains to:** Project

#### 14.21 EngagementPlan
- **Owner:** CSO or PLM
- **Location:** `<product-repo>/docs/3pao-engagement-plan.md`
- **Produced by:** CSO/PLM (when engaging domain experts or consultants)
- **Pertains to:** Project

#### 14.22 GTMReadiness
- **Owner:** CMO
- **Location:** `<product-repo>/docs/planning/gtm-readiness-checklist.md`
- **Produced by:** CMO (pre-release)
- **Pertains to:** Project

#### 14.23 StakeholderUpdate
- **Owner:** CSO
- **Location:** `<product-repo>/docs/planning/stakeholder-update-YYYY-MM-DD.md`
- **Produced by:** CSO (periodic)
- **Pertains to:** ProductLine

#### 14.24 ActualVsEstimated
- **Owner:** Engineering
- **Location:** `<product-repo>/docs/sprint-N-actual-vs-estimated.md`
- **Produced by:** Engineering (end of sprint)
- **Pertains to:** Sprint

#### 14.25 UIStories
- **Owner:** CXO
- **Location:** `<product-repo>/docs/ui-stories.md`
- **Produced by:** CXO (per sprint with UX changes)
- **Pertains to:** Sprint

---

### Template Artifacts

#### 14.26 ProjectCharterTemplate
- **Owner:** ABE framework
- **Location:** `abe/docs/charters/project-charter-template.md`
- **Used by:** CSO + PLM + COO when drafting charters

#### 14.27 PLMTemplate
- **Owner:** ABE framework
- **Location:** `abe/workflows/cos/cso/plm-template.md`
- **Used by:** CSO when instantiating new PLMs

#### 14.28 AgentRosterTemplate
- **Owner:** ABE framework
- **Location:** `abe/docs/agent-roster.md`
- **Used by:** Instance when creating agent roster

#### 14.29 IssueTemplate
- **Owner:** ProductLine
- **Location:** `<product-repo>/.github/ISSUE_TEMPLATE/epic.md`
- **Used by:** PLM + Engineering when creating GitHub issues

---

### Workflow Artifacts

#### 14.30 AgentWorkflow
- **Owner:** ABE framework + Instance
- **Location:** `abe/workflows/<role>/workflow.md` or `<instance>/workflows/<role>/<product>/workflow.md`
- **Defines:** How an agent operates, their decision authority, and escalation paths

#### 14.31 ProjectLifecycleWorkflow
- **Owner:** ABE framework
- **Location:** `abe/workflows/agent-workflows.md`
- **Defines:** The end-to-end project lifecycle from planning to close

#### 14.32 ReportingProtocol
- **Owner:** ABE framework
- **Location:** `abe/governance/reporting-protocol.md`
- **Defines:** Meeting cadence, escalation framework, decision authority matrix

---

### Skill Artifacts

#### 14.33 PLMInstantiate
- **Owner:** ABE framework
- **Location:** `abe/skills/plm-instantiate/SKILL.md`
- **Used by:** CSO when creating new product lines

#### 14.34 HumanCounterpart
- **Owner:** ABE framework (optional instance override)
- **Location:** `abe/skills/human-counterpart/SKILL.md`
- **Used by:** When binding humans to agent counterparts

---

### Config Artifacts

#### 14.35 PLMInstances
- **Owner:** CSO
- **Location:** `<instance>/plm-instances.json`
- **Contents:** Registry of all product lines, PLMs, repos, token delegations
- **Updated by:** CSO (when PLMs are instantiated or decommissioned)

#### 14.36 HumanCounterpartBindings
- **Owner:** CoS
- **Location:** `<instance>/human-counterpart-bindings.json`
- **Contents:** Mapping of humans to their agent counterparts
- **Updated by:** CoS (when org changes)

#### 14.37 BranchBacklog
- **Owner:** PLM
- **Location:** `<product-repo>/BACKLOG.md`
- **Contents:** Prioritized backlog of stories not yet assigned to sprints
- **Updated by:** PLM (continuous)

#### 14.38 ReportingStructure
- **Owner:** CoS
- **Location:** `<instance>/docs/agent-roster.md` (human-readable) + derived from Agent `reports_to` relationships (machine-readable)
- **Contents:** The complete agent reporting tree — every agent, their role code, persona name, direct reports, human counterpart (if any), and roll-up chain to the CEO
- **Updated by:** CoS (when agents are spawned, decommissioned, or reporting lines change)
- **Formats:** Markdown table + ASCII tree (human-readable); the canonical source of truth is the set of Agent `reports_to` relationships
- **Pertains to:** Organization

---

### Framework Artifacts

#### 14.38 ABEReadme
- **Owner:** ABE framework
- **Location:** `abe/README.md`
- **Contents:** Framework overview, org chart, key rules, adoption guide

#### 14.39 InstanceReadme
- **Owner:** Instance
- **Location:** `<instance>/README.md`
- **Contents:** Instance overview, leadership, agents, product lines, config

#### 14.40 AgentWorkflowsIndex
- **Owner:** ABE framework
- **Location:** `abe/workflows/all_agents/README.md`
- **Contents:** Cross-cutting processes, resource reporting, escalation

#### 14.41 Ontology
- **Owner:** ABE framework
- **Location:** `abe/docs/ontology.md` + `abe/docs/ontology-diagram.html`
- **Contents:** Formal ontology of the ABE system

---

## Relationship Summary

```
Organization
  ├── led_by → Human
  ├── employs → Human (*)
  ├── has_agents → Agent (*)
  ├── has_product_lines → ProductLine (*)
  ├── has_projects → Project (*)
  └── has_reporting_structure → ReportingStructure (1)

Human
  ├── counterpart_of → Agent (0..1)
  └── served_by → ExecutiveAssistant (0..1)

Agent
  ├── reports_to → Agent (1)
  ├── manages → Agent (*)
  ├── counterpart_of → Human (0..1)
  ├── monitors → Project (*)
  ├── executes → Project (*)
  ├── owns → Artifact (*)
  ├── spawns → SubAgent (*)
  └── rolls_up_to → Human (1)

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
  ├── has_charter → Charter (1)
  ├── has_lessons → LessonsLearned (0..1)
  ├── scope → Epic (*)              [subset of roadmap]
  ├── scope → Story (*)             [stories within scoped epics]
  └── slice_of → Roadmap (1)         [roadmap from which scope was selected]

Sprint
  ├── part_of → Project (1)
  ├── contains_stories → Story (*)
  ├── contains_pods → Pod (*)
  ├── has_plan → SprintPlan (0..1)
  ├── has_report → SprintReport (0..1)
  ├── has_cso_review → CSOReview (0..1)
  ├── has_coo_readiness → COOReadiness (0..1)
  ├── has_evm_report → EVMReport (0..1)
  └── has_actuals → ActualVsEstimated (0..1)

Story
  ├── part_of → Sprint (0..1)
  ├── part_of_epic → Epic (0..1)
  └── assigned_to → Pod (0..1)

Pod
  ├── spawned_by → Engineering (1)
  └── works_on → Story (*)

SubAgent
  ├── spawned_by → Engineering (1)
  └── consumes_allocation_from → Engineering (1)

Artifact
  ├── owned_by → Agent (1)
  ├── reviewed_by → Agent (*)
  └── pertains_to → Entity (0..1)
```

---

## Axioms (Invariants)

### Organizational Invariants (R1)
1. CoS always exists — every ABE has exactly one CoS
2. CSO always exists — every ABE has exactly one CSO
3. Reporting tree is acyclic
4. Every agent rolls up to the CEO through at most 3 hops
5. Only CoS and CSO have direct human counterparts
6. The reporting structure artifact reflects the current state of all Agent `reports_to` relationships
7. EAs appear in the reporting structure documentation but have no `reports_to` edge

### EA Invariants (R2)
8. EAs are outside the reporting tree
9. Each EA serves exactly one human
10. EA persona name matches human name initial

### Product Invariants (R3)
11. Each product line has exactly one PLM
12. Each product line has exactly one roadmap
13. Roadmap horizon is 3-5 sprints

### Project Invariants (R4)
14. Every project belongs to exactly one product line
15. Every project has exactly one charter
16. Engineering executes every project
17. PLM monitors every non-strategic project
18. CSO monitors every strategic project
19. Projects follow lifecycle: planning → active → review → closing → closed
20. Every project's scope is a subset of its product line's roadmap (slice_of exactly 1 Roadmap)
21. Project scope epics are selected from the roadmap during planning (Phase 1) and fixed at charter approval
22. Roadmap changes after charter approval do not retroactively alter project scope
23. Every story in a project's scope belongs to exactly one of the project's scoped epics (or is a cross-epic story directly in the scope)

### Sprint Invariants (R5)
24. Sprints belong to exactly one project
25. Stories belong to at most one sprint
26. Sprint execution plan is owned by Engineering
27. Sprint review includes Engineering, PLM, CSO, COO, CoS

### Story Invariants (R6)
28. Every story has a unique story_id
29. Stories are estimated by Engineering
30. Stories are validated by PLM against acceptance criteria
31. Every story must meet the Standard Definition of Done

### Sub-Agent Invariants (R7)
32. All sub-agent resource consumption is debited from Engineering's allocation
33. Sub-agents are spawned only by Engineering
34. Sub-agent lifecycle: spawned → executing → review → integrated → closed

### Artifact Invariants (R8)
35. Every artifact has exactly one owner
36. Artifacts are only updated by their owner
37. Sprint artifacts live in the product line repo
38. Instance config artifacts live in the instance repo
39. Framework artifacts live in the ABE repo
40. Charter is approved before project execution begins
41. Lessons learned is produced at project close
42. EVM reports are produced for every sprint
43. CSO review is produced for every sprint plan
44. COO readiness is produced before every sprint execution
45. Reporting structure is updated whenever agents are spawned, decommissioned, or reporting lines change

### Resource Invariants (R9)
46. Every agent reports resource consumption to CRO
47. CoS sets hard limits at ≤80% of available
48. No agent exceeds hard limits without CoS approval

---

## State Machines

### Project
```
  planning ──charter_approved──▶ active ──all_sprints_done──▶ review
     │                              │                           │
   cancelled                    cancelled                   closing
                                                               │
                                                              closed
```

### Sprint
```
  planning ──sprint_zero──▶ zero ──begins──▶ executing ──review──▶ complete
                                                                        │
                                                                     (next)
```

### Story
```
  backlog ──planned──▶ planned ──assigned──▶ in_progress ──submitted──▶ review ──▶ done
                                                                              │
                                                                           deferred
```

### Sub-Agent
```
  spawned ──work──▶ executing ──ready──▶ review ──approved──▶ integrated ──▶ closed
                                              │
                                           rejected ──▶ executing
```

### Charter
```
  draft ──circulated──▶ reviewing ──approved──▶ (project active) ──▶ superseded
                 │
              rejected ──▶ draft
```

---

## File Location Conventions

### ABE Framework Repo (`abe/`)
| Artifact | Pattern |
|---|---|
| Framework README | `abe/README.md` |
| Agent roster template | `abe/docs/agent-roster.md` |
| Charter template | `abe/docs/charters/project-charter-template.md` |
| PLM template | `abe/workflows/cos/cso/plm-template.md` |
| Agent workflows | `abe/workflows/<role>/workflow.md` |
| Project lifecycle | `abe/workflows/agent-workflows.md` |
| Cross-cutting processes | `abe/workflows/all_agents/README.md` |
| Reporting protocol | `abe/governance/reporting-protocol.md` |
| Skills | `abe/skills/<skill>/SKILL.md` |
| Ontology | `abe/docs/ontology.md` |

### Instance Repo (`<instance>/`)
| Artifact | Pattern |
|---|---|
| Instance README | `<instance>/README.md` |
| Agent roster | `<instance>/docs/agent-roster.md` |
| PLM instances registry | `<instance>/plm-instances.json` |
| Human counterpart bindings | `<instance>/human-counterpart-bindings.json` |
| PLM workflow (per product) | `<instance>/workflows/plm/<product>/workflow.md` |
| Cross-cutting reference | `<instance>/workflows/all_agents/README.md` |
| Token storage | `<instance>/.plm-tokens/` (git-ignored) |
| Skills (instance override) | `<instance>/skills/<skill>/SKILL.md` |
| Reporting structure | `<instance>/docs/agent-roster.md` |

### Product Line Repo (`<product-repo>/`)
| Artifact | Pattern |
|---|---|
| Product README | `<product-repo>/README.md` |
| Product roadmap | `<product-repo>/docs/product-roadmap.md` |
| Product spec | `<product-repo>/docs/product-spec.md` |
| Personas | `<product-repo>/docs/personas.md` |
| Competitive analysis | `<product-repo>/docs/competitive-gap-analysis.md` |
| Branch backlog | `<product-repo>/BACKLOG.md` |
| Project charters | `<product-repo>/docs/charters/<project-id>.md` |
| Sprint plans | `<product-repo>/docs/sprint-N-execution-plan.md` |
| Sprint reports | `<product-repo>/docs/sprint-N-report.md` |
| CSO reviews | `<product-repo>/docs/sprint-reviews/cso/sprintN-cso-review.md` |
| COO readiness | `<product-repo>/docs/sprint-reviews/coo/sprintN-operational-readiness.md` |
| ADRs | `<product-repo>/docs/architecture-decisions/NNN-title.md` |
| Spike reports | `<product-repo>/docs/spike-N-title.md` |
| Test coverage | `<product-repo>/docs/test-coverage-analysis.md` |
| UX analysis | `<product-repo>/docs/ux-analysis.md` |
| User manual | `<product-repo>/docs/user-manual.md` |
| API reference | `<product-repo>/docs/api-reference.md` |
| Installation guide | `<product-repo>/docs/installation.md` |
| Developer onboarding | `<product-repo>/docs/developer-onboarding.md` |
| Lessons learned | `<product-repo>/docs/lessons-learned/<project-id>.md` |
| EVM reports | `<product-repo>/docs/sprint-reviews/cso/sprintN-cso-review.md` (embedded) |
| Epic deep dives | `<product-repo>/docs/epic-N-deep-dive.md` |
| Gap analyses | `<product-repo>/docs/epic-N-gap-analysis.md` |
| Planning artifacts | `<product-repo>/docs/planning/<artifact>.md` |
| Issue templates | `<product-repo>/.github/ISSUE_TEMPLATE/` |
