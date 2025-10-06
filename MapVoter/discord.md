---
layout: default
title: Discord Integration
parent: MapVoter
nav_order: 5
---

# Discord Integration
{: .no_toc }

## Setup Steps
1. Create application + bot in Developer Portal
2. Enable Privileged Intents (Server Members, Message Content)
3. Invite bot with required perms (Read, Send, Embed, Add Reactions, Manage Messages)
4. Insert bot token into `"Discord Apikey"` in config
5. Provide channel IDs (enable Developer Mode → Copy ID)

## Core Settings
```json
"Discord Settings": {
  "Enable Discord bot (true/false)": true,
  "Vote Channel id": "",
  "Winning Map Channel id": "",
  "Mention role on vote start and end": "@everyone",
  "Create new thread for every vote": false,
  "Discord Command Prefix": "!"
}
```

## Command Role Restriction Example
```json
"Discord Command Role Assignment (Empty = All roles can use command.)": {
  "generate": ["Admin", "Moderator"],
  "mapwipe": ["Admin"],
  "bpwipe": ["Admin"],
  "update": ["Admin"],
  "stopvoting": ["Admin", "Moderator"]
}
```
Empty array = public command.

## Channels Mapping
| Purpose | Field |
|---------|-------|
| Vote embeds | `Vote Channel id` |
| Winning map announcement | `Winning Map Channel id` |
| Logs (if enabled) | `Discord Logs Channel Id` |

## Tips
- Use `||@everyone||` to visually show role but suppress actual ping if desired
- Threads keep chat clean during heavy voting
- Use `Single Embed?` true for compact multi-map embed

## Troubleshooting
| Problem | Fix |
|---------|-----|
| Bot silent | Token invalid / missing intents |
| Reactions not added | Missing Manage Messages permission |
| Commands ignored | Prefix mismatch or role restriction |

---
Full legacy details: [/MapVoter/Config-Guide.html#discord-settings](/MapVoter/Config-Guide.html#discord-settings)
