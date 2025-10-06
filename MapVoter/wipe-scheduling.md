---
layout: default
title: Auto Wipe & Voting
parent: MapVoter
nav_order: 4
---

# Auto Wipe & Voting Logic
{: .no_toc }

## Concepts
- Forced Wipe: First Thursday each month (Facepunch)
- Map Wipe schedule: Days after forced wipe for map-only wipes
- BP Wipe schedule: Days after forced wipe for blueprint wipes (0 = forced wipe day)

## Example Weekly Cycle
```
"Map Wipe schedule": [7,14,21,28]
"BP Wipe schedule": [0]
```
All Thursdays = map wipe; first includes BP.

## Voting Timing Rule
Vote start time must be at least 2 hours before wipe time.
Voting ends at: `Vote Start Time + Stop voting after (minutes)`.

## Common Patterns
| Pattern | Map Schedule | BP Schedule | Notes |
|---------|--------------|-------------|-------|
| Weekly | [7,14,21,28] | [0] | Standard weekly | 
| Bi-Weekly | [14,28] | [0] | Two per month |
| Monthly | [] | [0] | Forced only |
| 10-day | [10,20] | [0] | Three per month |

## Selecting Schedules
Use the (legacy) interactive picker in the old guide for now.

## Quick Voting Scenarios
| Scenario | Config Snippet |
|----------|----------------|
| Vote starts 1 day before, 1h window wipe day | `"Start voting X days before wipe": 1`, `"Vote start": 17:00`, `Stop after: 60` |
| Same day short vote | `"Start voting X days before wipe": 0`, `"Vote start": 10:00`, `Stop after: 60` |

## Blueprint Strategy
- Frequent BP wipes = fresh progression
- Rare BP wipes = player retention of tech
- Hybrid: BP wipe every 2nd forced wipe (`[0,35]` with weekly map wipes)

## Troubleshooting
| Issue | Check |
|-------|-------|
| Vote never starts | Auto Vote + Auto Wipe enabled, times valid |
| Wipe skipped | Time zone mismatch, schedule arrays empty |
| Vote ends after wipe | Stop duration too large vs start time |

---
Legacy full details: [/MapVoter/Config-Guide.html#auto-wipe](/MapVoter/Config-Guide.html#auto-wipe)
