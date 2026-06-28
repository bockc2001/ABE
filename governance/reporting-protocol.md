# Reporting Protocol

## Human Counterpart Relationship

Each agent has a **human counterpart** — the primary human they work with. This is an active, direct working relationship encompassing:

- **Direct access** — no intermediaries needed for day-to-day collaboration
- **Co-creation** — collaborating on artifacts, plans, strategies, and deliverables
- **Oversight & guidance** — providing direction, feedback, and strategic alignment
- **Partnership** — the human is the agent's go-to for decisions, input, and review

Only **CoS** and **CSO** have direct human counterparts. All other agents roll up the reporting tree to reach human support.

## Meeting Cadence

| Meeting | Participants | Frequency |
|---|---|---|
| Daily standup | CoS + CEO | Daily |
| Weekly full leadership | CoS + all C-suite | Weekly |
| Weekly ops sync | CoS + COO + direct reports | Weekly |
| Weekly finance sync | CoS + CFO + CRO | Weekly |
| Marketing sync | CoS + CMO | As needed |
| Strategy sync | CSO + CEO | Weekly |
| Product review | CSO + PLM + CEO | Per sprint |
| Sprint review | Engineering + PLM + CSO + COO + CoS | Per sprint |

## Escalation Framework

### Standard Escalation Path

```
Domain issue → Domain lead → CoS → CEO
```

### Cross-Functional Escalation

```
Cross-functional issue → Any involved agent → CoS (convenes) → CEO if unresolved
```

### Emergency Escalation

```
Material incident → Any agent → CoS + CEO (immediate, simultaneous notification)
```

### Escalation Triggers

| Trigger | Escalate To | Timing |
|---|---|---|
| Cross-functional blocker | CoS | Within 4 hours |
| Resource limit exceeded | CoS + CRO | Immediate |
| Security incident | CoS + CEO | Immediate |
| Strategic pivot needed | CSO + CEO | Within 24 hours |
| Project at risk (SPI/CPI < 0.9) | CSO → CoS | Next sprint review |
| Project critical (SPI/CPI < 0.75) | CSO → CoS → CEO | Within 48 hours |

## Decision Authority Matrix

| Decision Type | Authority | Approval Required |
|---|---|---|
| Day-to-day operations | Domain lead | — |
| Cross-functional decisions | CoS (convenes) | CEO if strategic |
| Resource allocation | CoS (sets limits) | CEO for changes |
| New product line | CSO (recommends) | CEO |
| Hiring (human or agent) | Domain lead + CoS | CEO |
| Budget changes | CFO (recommends) | CEO |
| Scope changes (material) | PLM/CSO | CSO + CEO if strategic |
| Rebaselining | CCB (recommends) | CEO |
| Project cancellation | CSO (recommends) | CEO |

## Resource Reporting

### Mechanism

Every agent files a **weekly resource report** to the CRO every Sunday at 20:00 UTC. The report is a JSON file committed to the instance repo:

```
turingdynamics/resources/YYYY-W<week-number>.json
```

### Report Schema

```json
{
  "agent": "miles",
  "role": "PLM",
  "period": {
    "start": "2026-06-22",
    "end": "2026-06-28"
  },
  "actuals": {
    "tokens": { "total": 150000, "daily": { "2026-06-22": 20000, "2026-06-23": 25000 } },
    "compute": { "total_ms": 120000, "daily": {} },
    "requests": { "total": 45, "daily": {} }
  },
  "requests": {
    "tokens": { "total": 200000, "daily": { "2026-06-29": 30000, "2026-06-30": 30000 } },
    "compute": { "total_ms": 150000, "daily": {} },
    "requests": { "total": 60, "daily": {} }
  },
  "monthly": {
    "current": { "tokens": 350000, "compute_ms": 270000, "requests": 105 },
    "next": { "tokens": 400000, "compute_ms": 300000, "requests": 120 }
  },
  "tasks_completed": 3,
  "tasks_planned": 4
}
```

### Three Data Sets (per agent)

| Report | Period | Content |
|---|---|---|
| **Historical Actuals** | Last 7 days (by day) | Actual tokens consumed, compute time, API requests |
| **Near-term Requests** | Next 7 days by day | Projected tokens, compute, requests needed |
| **Monthly Aggregates** | Current month + next month | Aggregate resource needs |

### Roll-Up Chain

1. **Each agent** → files weekly JSON report to `turingdynamics/resources/`
2. **CRO (Rick)** → aggregates all reports, calculates totals, identifies bottlenecks
3. **CoS (Craig)** → reviews CRO aggregate, sets HARD per-agent limits at ≤80% of total available
4. **CEO** → notified only if limits need to change or resources are exceeded

### Limit Enforcement

- **HARD limit** = 80% of remaining budget for the period
- **SOFT limit** = 90% of remaining budget — triggers warning to agent + CoS
- **Exceedance** = agent MUST stop non-essential work and request additional budget from CoS
- **No exceedance without CoS approval.** Period.

### Initial Budget (per week)

| Resource | Total | Per-Agent Max (80%) |
|---|---|---|
| Tokens | 2,000,000 | 400,000 (2 agents active = 800K allocated) |
| Compute | 2 hours wall-clock | 30 min per agent |
| API requests | 1,000 | 200 per agent |

> Budget is set by CEO, adjusted weekly based on CRO reports.

### Non-Compliance

- Agent exceeds SOFT limit → warning issued, must justify within 4 hours
- Agent exceeds HARD limit → agent must idle until CoS approves additional budget
- 3+ violations in a month → CEO review required

## Communication Norms

- **Async-first:** Default to written updates; meetings for decisions and alignment
- **Decision log:** All decisions with material impact are logged with: decision, rationale, decision-maker, date
- **Status updates:** Each agent provides periodic status to their reporting lead
- **No silent blockers:** If blocked, escalate within 4 hours — don't wait to be asked
