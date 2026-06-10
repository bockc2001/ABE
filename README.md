# ABE — Automated Business Entity

## What Is an ABE?

An Automated Business Entity is an AI-managed business with human leadership and a full executive team of named agents. The ABE framework defines the **generic organizational structure, governance, workflows, and agent roles** that apply to any ABE instance.

An ABE instance (e.g., TuringDynamics) provides the **specifics**: resources, strategy, product lines, and domain configuration.

---

## Org Chart — Agents

```
Human Leadership
└── CoS (Chief of Staff)                  ← Primary coordination interface
    ├── CFO (Chief Financial Officer)
    ├── CRO (Chief Resource Officer)
    ├── COO (Chief Operating Officer)
    │   ├── Engineering
    │   ├── Production
    │   └── Logistics
    ├── CMO (Chief Marketing Officer)
    ├── CSO (Chief Strategy Officer)
    │   └── PLMs (Product Line Managers)   ← One per product line
    ├── GC (General Counsel)
    └── CXO (Chief Experience Officer)

Executive Assistants (outside agent hierarchy)
└── One EA per human leader
```

---

## Org Chart — Human-Agent Pairings

Only **CoS** and **CSO** have direct human counterparts. All other agents roll up the reporting tree to reach human support.

| Agent | Human Counterpart | Direct? |
|---|---|---|
| CoS | CEO / Primary Leader | ✅ Direct |
| CSO | CEO / Primary Leader | ✅ Direct |
| CFO | — | Via CoS |
| CRO | — | Via CoS |
| COO | — | Via CoS |
| Engineering | — | Via COO → CoS |
| Production | — | Via COO → CoS |
| Logistics | — | Via COO → CoS |
| CMO | — | Via CoS |
| GC | — | Via CoS |
| CXO | — | Via Cos |
| PLMs | — | Via CSO → CEO |

---

## Naming Convention

All agents use **short American names** where the **first letter matches the role's initial**:
- CoS → Craig, CFO → Fred, CRO → Rick, COO → Owen, CMO → Mike, CSO → Sam, GC → Greg, CXO → Xavier

PLM names are **not** constrained by this convention — they are product-specific.

EA names match the **first letter of their human counterpart's name**.

---

## Repository Structure

```
abe/                          ← Generic ABE framework (this repo)
├── README.md                 — This file
├── docs/                     — Architecture, agent specs, templates
│   ├── agent-roster.md       — Full agent roster template
│   └── charters/             — Project charter templates
├── governance/               — Decision frameworks, escalation, reporting
│   └── reporting-protocol.md — Communication flows and authority matrix
├── workflows/                — Agent workflow definitions
│   ├── all_agents/           — Cross-cutting processes, resource reporting
│   ├── cos/                  — CoS + C-suite workflows
│   ├── coo/                  — COO + ops workflows
│   ├── ea/                   — Executive assistant workflows
│   └── skills/               — Reusable skills
└── skills/                   — Standalone skills

<instance>/                   ← ABE instance (e.g., turingdynamics/)
├── README.md                 — Instance overview, human leadership, product lines
├── docs/                     — Instance-specific docs
├── workflows/
│   ├── plm/                  — Product Line Manager workflows (per product)
│   └── reviews/              — Sprint reviews, operational readiness
├── plm-instances.json        — PLM registry
└── .plm-tokens/              — Git-ignored token storage
```

---

## Key Rules

1. **CoS is the primary coordination interface** for all domain agents.
2. **Only CoS and CSO** have direct human counterparts. All other agents roll up the tree.
3. **EAs are outside the agent reporting structure** — each EA reports directly to their human counterpart.
4. **PLMs are spawned per product line** and report to CSO.
5. **Product-specific content lives in the product line repo**, not in the ABE framework or instance repos.
6. **The ABE framework is generic** — no product-specific or instance-specific content.

---

## What Is a Project?

A **project** is a discrete body of work to build, extend, or improve a product. It has:

- A **charter** — defines scope, objectives, resources, and success criteria
- A **sprint cadence** — work is planned and executed in sprints
- A **defined end** — a project closes when its charter objectives are met, or when it is cancelled
- **Ownership** — Engineering executes, PLM monitors, CSO provides strategic oversight

Projects are the primary unit of execution in an ABE. Product lines are made up of successive projects that evolve the product over time.

### Project Lifecycle

```
Planning → Charter Approval → Sprint Zero → Sprint Execution → Sprint Review → Release → Close
```

See `workflows/agent-workflows.md` for the full lifecycle.

### Project vs. Product Line

| | Product Line | Project |
|---|---|---|
| **Scope** | Entire product (ongoing) | Discrete body of work (bounded) |
| **Duration** | Indefinite | Finite (charter to close) |
| **Owner** | PLM | Engineering (execution), PLM (monitoring) |
| **Output** | Evolving product | Shipped increment |

A product line may have multiple concurrent projects (e.g., one per major feature area) or a single sequential project pipeline.

---

## Adopting the ABE Framework

To create a new ABE instance:

1. Create a new repo for the instance (e.g., `turingdynamics/`)
2. Reference this ABE repo as the organizational template
3. Define instance-specific configuration: human leadership, resources, strategy, product lines
4. Spawn PLMs for each product line (each PLM gets their own product line repo)
5. Customize workflows as needed while maintaining ABE compliance

---

## Status

⚙️ **Active Development** — Defining the generic ABE framework.
