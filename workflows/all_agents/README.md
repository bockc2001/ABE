# All Agents — Workflow Index

## Shared Skills

These skills are available to all agents (loaded from `skills/`):

| Skill | Used by | Purpose |
|-------|---------|---------|
| **engineering-pod-execution** | Engineering (Ethan) | Execute sprints via ephemeral pods |
| **cross-functional-coordination** | CoS (Craig) | Convene agents, track decisions, escalate blockers |
| **agent-onboarding** | CoS (Craig) | Onboard new agents into the ABE instance |
| **plm-instantiate** | CSO (Sam) | Create a new PLM for a product line |

## Reporting Tree

```
Human Leadership
└── CoS
    ├── CFO
    ├── CRO
    ├── COO
    │   ├── Engineering
    │   ├── Production
    │   └── Logistics
    ├── CMO
    ├── CSO
    │   └── PLMs (one per product line)
    ├── GC
    └── CXO

Executive Assistants (outside agent hierarchy)
└── One EA per human leader
```

> **Note:** Executive Assistants (EAs) are **outside the agent reporting structure**. Each EA serves a human leader directly and has no reporting relationship to CoS or any agent in the domain hierarchy.

## Resource Reporting (All Roles)

Every role reports three data sets to the CRO:

| Report | Period | Content |
|---|---|---|
| **Historical Actuals** | Last 7 days (by day) | Actual tokens, compute, requests consumed |
| **Near-term Requests** | Next 7 days (by day) | Projected tokens, compute, requests needed |
| **Monthly Aggregates** | Current month + next month | Aggregate resource requests per calendar month |

**Limit enforcement:** CRO aggregates → CoS sets HARD limits at ≤80% of total available. No exceedance without CoS approval.

## Cross-Cutting Processes

### Resource Limit Change
1. Role identifies need for limit increase
2. Submits request to CoS with justification
3. CoS reviews with CRO for resource availability
4. If available → CoS adjusts limit (≤80% cap maintained)
5. If not available → CoS explores reallocation from underutilized roles
6. CEO approves any limit changes

### New Agent Onboarding
1. CEO + domain lead identify need
2. CoS coordinates onboarding (access, tools, counterpart assignment)
3. New agent receives org structure, reporting protocol, resource obligations
4. CRO adds new agent to resource reporting
5. CoS sets initial hard limits

### Incident Response
1. Detecting role assesses severity
2. Immediate notification to human counterpart + domain lead
3. Domain lead coordinates response
4. CoS notified for cross-functional incidents
5. CEO notified for material incidents
6. Post-incident review by domain lead
7. Preventive measures tracked to completion

### Cross-Functional Initiative
1. Initiating role identifies cross-functional need
2. Routes to CoS for coordination
3. CoS convenes relevant roles and domain leads
4. Shared plan developed with clear ownership
5. CoS tracks progress, escalates blockers to CEO
