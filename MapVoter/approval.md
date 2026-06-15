---
layout: default
title: Map Approval Workflow
parent: MapVoter
nav_order: 6
---

# Map Approval Workflow
{: .no_toc }

This page merges the approval pipeline documentation into the main MapVoter docs set.

## Overview

MapVoter can optionally pause the normal vote flow and review generated RustMaps results before the player vote starts.

- Auto Check evaluates map data against monument and numeric rules.
- Manual Approval posts map reviews to Discord and waits for an admin decision.
- Both modes can run together. Auto checks still run first, and manual review can override or approve flagged maps.

## How the pipeline works

1. RustMaps finishes a batch of generated maps.
2. The approval gate evaluates the maps.
3. Passing maps continue to the vote or review flow.
4. If Manual Approval is enabled, maps are posted to Discord for Approve / Reject actions.
5. Rejected maps can request regeneration, and the review continues until the timeout or regeneration cap is reached.

## Configuration keys

### Map Auto Check

```json
"Map Auto Check": {
  "Enabled": false,
  "Require Approval On Auto Fail": false,
  "Eligibility - Monument Rules": {
    "Must-Have Monuments": [],
    "Forbidden Monuments": []
  },
  "Eligibility - Numeric Rules": {
    "Islands": { "Min": -1, "Max": -1 },
    "Mountains": { "Min": -1, "Max": -1 },
    "Ice Lakes": { "Min": -1, "Max": -1 },
    "Rivers": { "Min": -1, "Max": -1 },
    "Lakes": { "Min": -1, "Max": -1 },
    "Canyons": { "Min": -1, "Max": -1 },
    "Oases": { "Min": -1, "Max": -1 }
  }
}
```

### Map Manual Approval

```json
"Map Manual Approval": {
  "Enabled": false,
  "Review Channel Id": "",
  "Timeout (seconds)": 1800,
  "Max Regeneration Attempts": 5,
  "Regeneration Timeout (ms)": 60000
}
```

## Toggle combinations

| Auto Check | Manual Approval | Result |
|---|---|---|
| Off | Off | Legacy vote flow only |
| On | Off | Auto filter maps before voting |
| Off | On | Review maps in Discord before voting |
| On | On | Auto filter + Discord review + optional regeneration |

## Rules to know

- Monument rules match the exact RustMaps monument type names.
- Numeric rules are inclusive and use `Min` / `Max` values.
- `-1` means “no limit” for a rule side.
- If the final approved set is empty, the vote does not start.

## Role permissions

The review buttons use the existing Discord command role system. Add the new role key below if you want to restrict approvals:

```json
"CmdRoles": {
  "mapvote": ["Admin", "Moderator"],
  "mapreview": ["Admin"]
}
```

If `mapreview` is missing, all roles may click the buttons. If it exists with at least one role name, only those roles can review.

## Troubleshooting highlights

- If the manual review channel ID is invalid, the review will be blocked.
- If all maps auto-fail, the vote will not start until rules are relaxed or an admin overrides the result.
- If the review timeout expires with no approved map set, the vote will not start.
- If the regeneration cap is hit, the current approved set will resolve the review.

## See also

- [Configuration](./configuration)
- [Discord Integration](./discord)
- [Legacy Full Guide](./Config-Guide.html)
