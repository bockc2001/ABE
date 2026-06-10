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

Every agent reports three data sets to the CRO:

| Report | Period | Content |
|---|---|---|
| **Historical Actuals** | Last 7 days (by day) | Actual tokens, compute, requests consumed |
| **Near-term Requests** | Next 7 days (by day) | Projected tokens, compute, requests needed |
| **Monthly Aggregates** | Current month + next month | Aggregate resource requests per calendar month |

**Limit enforcement:** CRO aggregates → CoS sets HARD limits at ≤80% of total available. No exceedance without CoS approval.

## Communication Norms

- **Async-first:** Default to written updates; meetings for decisions and alignment
- **Decision log:** All decisions with material impact are logged with: decision, rationale, decision-maker, date
- **Status updates:** Each agent provides periodic status to their reporting lead
- **No silent blockers:** If blocked, escalate within 4 hours — don't wait to be asked
