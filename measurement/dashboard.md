# Measurement Dashboard

Top-level KPI overview across all active and recent campaigns. Read this when you want a system-wide view before drilling into a specific campaign's retro.

## Active campaigns

| Campaign ID | Sprint | Day | Status | Primary KPI | vs Target |
|---|---|---|---|---|---|
| _(populated on /sprint-start)_ | | | | | |

## Last 4 sprints — leading indicators

| Sprint | Campaigns | Avg hook-2s | Avg hold-25 | Avg CTR | Best beat | Worst beat |
|---|---|---|---|---|---|---|
| _(populated on /retro)_ | | | | | | |

## Last 4 sprints — lagging indicators

| Sprint | Campaigns | Avg VTR | Avg CPA | Brand lift | Notes |
|---|---|---|---|---|---|
| | | | | | |

## Meta-system metrics (the OS itself)

These track whether the OS is getting better over time. The numbers should trend in the right direction across sprints.

| Sprint | Avg attempts per hero beat | Approval rate per beat | Time per asset | Credit cost per shipped asset |
|---|---|---|---|---|
| | | | | |

## Prompt version performance

For each agent, track which prompt versions have been in flight and how they performed.

### Showrunner
| Version | Sprints used | Bible accept rate | AI-native justification quality |
|---|---|---|---|
| v1 | _initial_ | — | — |

### DP
| Version | Sprints used | Avg approval rate | Avg attempts per beat | Face-filter rejection rate |
|---|---|---|---|---|
| v1 | _initial_ | — | — | — |

### Motion
| Version | Sprints used | On-time adaptation rate | Template reuse rate |
|---|---|---|---|
| v1 | _initial_ | — | — |

### Channel Strategist
| Version | Sprints used | Plan accuracy | Time data→decision |
|---|---|---|---|
| v1 | _initial_ | — | — |

### Analyst
| Version | Sprints used | Retros published on time | Cross-sprint patterns identified |
|---|---|---|---|
| v1 | _initial_ | — | — |

(Art Director and CGI tracked when invoked.)

## How this gets updated

The Analyst updates this dashboard at the end of every retro. The dashboard's job is to make trend lines visible — if approval rates are dropping or attempt counts are rising, the OS is regressing and the next retro should address it.

## Quarterly meta-review

Every 12 sprints, re-read this dashboard alongside the last 12 retros. Look for:

- Patterns the dashboard reveals that individual retros missed
- Prompt versions that should be promoted to canonical (replace `agents/{name}.md`)
- Methodology updates needed (write to `playbooks/methodology.md`)

The OS is supposed to compound. If the dashboard isn't trending up, something is broken and needs to be fixed at the system level.
