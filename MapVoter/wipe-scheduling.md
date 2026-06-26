---
layout: default
title: Auto Wipe & Voting
parent: MapVoter
nav_order: 4
description: Copy-paste wipe schedules and voting timing for MapVoter — weekly, bi-weekly, monthly, and custom, with worked calendars and timelines.
---

# Auto Wipe & Voting
{: .no_toc }

Ready-to-use wipe schedules and voting timing. Copy a config block, drop it into
`oxide/config/MapVoter.json`, and reload the plugin.
{: .fs-5 .fw-300 }

<details open markdown="block">
  <summary>On this page</summary>
{: .text-delta }
1. TOC
{:toc}
</details>

---

## How schedules work

Wipe days are defined **relative to the monthly forced wipe**, not to calendar dates.

- **Forced wipe** — the first Thursday of every month (set by Facepunch, ~19:00 UTC). MapVoter always wipes the map and blueprints on this day.
- **`Map Wipe schedule`** — days *after* the forced wipe to wipe the **map** only. `[7, 14, 21, 28]` = 7, 14, 21, 28 days later.
- **`BP Wipe schedule`** — days after the forced wipe to wipe **blueprints**. `0` means "on the forced wipe day".
- **`Wipe time`** — 24-hour clock, server time.

> The schedule arrays repeat every month. `[7, 14, 21, 28]` lands on every Thursday because the forced wipe is itself a Thursday.

---

## Live examples

Each block is a complete `Auto Wipe` section. Change the times to your server's timezone.

### Weekly — map every Thursday, BP once a month

The most common setup. Fresh map weekly; blueprints only at the monthly forced wipe.

```json
"Auto Wipe": {
  "Enable Auto Wipe (true/false)": true,
  "Map Wipe schedule": [7, 14, 21, 28],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

| Day | Wipes |
|-----|-------|
| 1st Thursday (forced) | Map **+** Blueprints |
| Every other Thursday | Map only |

### Bi-weekly — twice a month

```json
"Auto Wipe": {
  "Enable Auto Wipe (true/false)": true,
  "Map Wipe schedule": [14, 28],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

| Day | Wipes |
|-----|-------|
| 1st Thursday (forced) | Map **+** Blueprints |
| ~2 weeks later | Map only |

### Monthly — forced wipe only

```json
"Auto Wipe": {
  "Enable Auto Wipe (true/false)": true,
  "Map Wipe schedule": [],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

An empty `Map Wipe schedule` means no extra map wipes — just the monthly forced wipe.

### Custom — roughly every 10 days (3× a month)

```json
"Auto Wipe": {
  "Enable Auto Wipe (true/false)": true,
  "Map Wipe schedule": [10, 20],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

### Blueprint wipe every 2 months

Keep weekly map wipes, but only wipe blueprints every other month so players keep tech longer.

```json
"Auto Wipe": {
  "Enable Auto Wipe (true/false)": true,
  "Map Wipe schedule": [7, 14, 21, 28],
  "BP Wipe schedule": [0, 35],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

`35` is five weeks after the forced wipe — i.e. the *next* month's forced wipe day — so blueprints survive one full month before wiping.

---

## What a month looks like

Configuration: `"Map Wipe schedule": [7, 14, 21, 28]`, `"BP Wipe schedule": [0]`

January 2025 — the first Thursday is the 2nd, so every wipe lands on a Thursday:

```
Sun Mon Tue Wed Thu Fri Sat
              1  [ 2]  3   4     [ 2]  Forced wipe — Map + BP
  5   6   7   8  [ 9] 10  11     [ 9]  Map only   (+7 days)
 12  13  14  15  [16] 17  18     [16]  Map only   (+14 days)
 19  20  21  22  [23] 24  25     [23]  Map only   (+21 days)
 26  27  28  29  [30] 31         [30]  Map only   (+28 days)
```

> **Read it as:** forced wipe on the first Thursday (Map + BP), then map-only wipes 7 / 14 / 21 / 28 days later — every following Thursday. On the next month's first Thursday the cycle restarts with another Map + BP wipe.

---

## Voting timing

Auto Vote opens a map vote *before* a wipe so the winning map is ready when the server wipes.

> **Critical rule:** the vote **start time must be at least 2 hours before the wipe time**. Wipe at `19:00` → vote start `17:00` or earlier. Vote start `18:00` is too late and the vote won't run.

**When voting ends:** on wipe day at `Vote start time + Stop voting after (minutes)` — *not* relative to the wipe time.

### Recommended — 1-hour window, opens the day before

```json
"Auto Vote": {
  "Enable Auto Vote (true/false)": true,
  "Start voting X days before wipe": 1,
  "Vote start (HH:mm) 24-hour clock": "17:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 60
  }
}
```

| Day | Time | Event |
|-----|------|-------|
| Wednesday | 17:00 | Voting starts |
| Wed 17:00 – Thu 17:59 | | Players vote (~25 h) |
| Thursday | 18:00 | Voting ends (17:00 + 60 min) |
| Thursday | 19:00 | Server wipes to the winning map |

### Same-day — short 1-hour vote

```json
"Auto Vote": {
  "Enable Auto Vote (true/false)": true,
  "Start voting X days before wipe": 0,
  "Vote start (HH:mm) 24-hour clock": "10:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 60
  }
}
```

| Day | Time | Event |
|-----|------|-------|
| Thursday | 10:00 | Voting starts |
| Thursday | 10:00 – 10:59 | Players vote |
| Thursday | 11:00 | Voting ends (10:00 + 60 min) |
| Thursday | 19:00 | Server wipes to the winning map |

---

## Quick reference

| Pattern | `Map Wipe schedule` | `BP Wipe schedule` |
|---------|--------------------|--------------------|
| Weekly | `[7, 14, 21, 28]` | `[0]` |
| Bi-weekly | `[14, 28]` | `[0]` |
| Monthly | `[]` | `[0]` |
| Every ~10 days | `[10, 20]` | `[0]` |
| Weekly map, BP every 2 months | `[7, 14, 21, 28]` | `[0, 35]` |

---

## Troubleshooting

| Symptom | Check |
|---------|-------|
| Vote never starts | Both **Auto Vote** and **Auto Wipe** enabled; vote start ≥ 2 h before wipe time |
| Vote ends after the wipe | `Stop voting after (minutes)` is too large for the vote start time |
| A wipe was skipped | Timezone mismatch, or the schedule array is empty for that day |
| BPs wiped when they shouldn't | Remove the day from `BP Wipe schedule`; check `Wipe BPs at forced wipe day` |

---

Need every option spelled out? See the [Full Configuration Guide](./full-guide#auto-wipe).
