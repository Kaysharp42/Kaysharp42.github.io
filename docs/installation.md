---
layout: default
title: Installation
nav_order: 2
---
# Installation

Follow these steps to install the MapVoter plugin on your Rust server.

## Prerequisites

- Oxide / uMod installed
- Access to your server's `oxide/plugins/` and `oxide/config/` directories
- (Optional) RustMaps.com subscription (for API access)
- (Optional) Discord bot token (for Discord integration)

## 1. Download Plugin

Purchase and download the latest `MapVoter.cs` from Codefling:
- https://codefling.com/plugins/map-voter-and-auto-wipe-script

Place `MapVoter.cs` into: `oxide/plugins/`

## 2. Generate Default Config

Start (or restart) your server to generate the config file:
```
oxide/plugins/MapVoter.cs -> loads -> creates oxide/config/MapVoter.json
```

## 3. Locate Config File

Edit: `oxide/config/MapVoter.json`

Populate at minimum:
```json
{
  "RustMaps Settings": {
    "Map size": 3500,
    "RustMaps API key": "YOUR_API_KEY_HERE",
    "RustMaps ORG ID": "YOUR_ORG_ID_HERE"
  },
  "Auto Wipe": {
    "Enable Auto Wipe (true/false)": true,
    "Map Wipe schedule": [7, 14, 21, 28],
    "BP Wipe schedule": [0],
    "Wipe time (HH:mm) 24-hour clock": "19:00"
  }
}
```

## 4. Set Up RustMaps API

1. Visit https://rustmaps.com/user/profile
2. Copy your API Key & Organization ID
3. Paste into config

## 5. (Optional) Configure Discord Bot

1. Create a bot at https://discord.com/developers
2. Enable Message Content intent
3. Invite bot with permissions:
   - Read / Send Messages
   - Embed Links / Add Reactions
   - Manage Messages (for reaction voting)
4. Put bot token in config under `Discord Settings`

## 6. Assign Permissions

Grant admin controls:
```
oxide.grant group admin mapvoter.admin
```
Restrict voting (optional):
```
oxide.grant group default mapvoter.vote
```

## 7. Verify Operation

In server console:
```
oxide.reload MapVoter
MapVoter.generate 5 0 60
```
Players can now `/mvote`.

## Next Steps

- Configure wipe logic: [Auto Wipe & Voting](/docs/wipe-scheduling)
- Customize UI Theme: [Configuration](/docs/configuration#ui-theme-system)
- Set up Discord channels: [Discord Integration](/docs/discord)
