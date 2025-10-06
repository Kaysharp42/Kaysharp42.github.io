---
layout: default
title: Configuration
nav_order: 3
---
# Configuration

This page documents all configuration sections for MapVoter. The actual JSON file lives at:
```
oxide/config/MapVoter.json
```

> Tip: Keep version control of your config (commit changes) so you can diff updates between plugin versions.

## Minimum Required Config
```json
{
  "RustMaps Settings": {
    "Map size": 3500,
    "RustMaps API key": "YOUR_API_KEY_HERE",
    "RustMaps ORG ID": "YOUR_ORG_ID_HERE"
  },
  "Auto Wipe": {
    "Enable Auto Wipe (true/false)": true,
    "Map Wipe schedule": [7,14,21,28],
    "BP Wipe schedule": [0],
    "Wipe time (HH:mm) 24-hour clock": "19:00"
  }
}
```

## Sections
1. Config Version
2. Theme (UI Theme System)
3. Commands
4. Rewards
5. Fun Kit
6. Options
7. RustMaps Settings
8. Discord Settings
9. Auto Vote
10. Auto Wipe
11. Plugins Data Wipe

(Full expanded configuration content coming soon – see legacy [full guide](/MapVoter/Config-Guide.html) for now.)

## UI Theme System
Refer to legacy guide for full tables until migrated.

Example override:
```json
"Theme": {
  "Colors": { "Primary": "#2563eb", "Secondary": "#16a34a" },
  "Effects": { "EnableBlur": true, "EnableShadows": true }
}
```

## Auto Wipe
```
"Auto Wipe": {
  "Map Wipe schedule": [7,14,21,28],
  "BP Wipe schedule": [0],
  "Enable Auto Wipe (true/false)": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

See: [Wipe Scheduling](/docs/wipe-scheduling)

## Pending Migration
Large detailed sections (RustMaps Settings, Discord Settings, Theme tables, scenarios) will be ported from the legacy HTML progressively.

---
_Still missing something?_ View the full original guide: [/MapVoter/Config-Guide.html](/MapVoter/Config-Guide.html)
