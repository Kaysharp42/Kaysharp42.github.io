---
layout: default
title: MapVoter
nav_order: 2
has_children: true
permalink: /MapVoter/
---

# MapVoter
{: .no_toc .text-delta }

Official documentation for the MapVoter Rust server plugin - automate map voting, wipe scheduling, and Discord integration.
{: .fs-6 .fw-300 }

[Purchase on Codefling](https://codefling.com/plugins/map-voter-and-auto-wipe-script){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View Full Legacy Guide](./Config-Guide.html){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview

MapVoter is a comprehensive Rust server plugin that provides:

- **Automated Map Voting** - In-game UI + Discord bot integration
- **Flexible Wipe Scheduling** - Weekly, bi-weekly, monthly, or custom patterns
- **RustMaps Integration** - Procedural and custom map support via API
- **Discord Bot** - Full command suite, voting embeds, and notifications
- **Reward System** - ServerRewards points and Kit rewards for voters
- **Fun Kit Events** - Pre-wipe gear distribution system
- **Plugin Data Cleanup** - Automatic data wipe on schedule
- **Advanced UI Theming** - Colors, spacing, typography, and effects

---

## Quick Start

### Installation
1. Purchase from [Codefling](https://codefling.com/plugins/map-voter-and-auto-wipe-script)
2. Place `MapVoter.cs` in `oxide/plugins/`
3. Configure RustMaps API key in `oxide/config/MapVoter.json`
4. Set up wipe schedules

[Full Installation Guide →](./installation)

### Basic Configuration
```json
{
  "RustMaps Settings": {
    "Map size": 3500,
    "RustMaps API key": "YOUR_KEY",
    "RustMaps ORG ID": "YOUR_ORG"
  },
  "Auto Wipe": {
    "Enable Auto Wipe (true/false)": true,
    "Map Wipe schedule": [7, 14, 21, 28],
    "BP Wipe schedule": [0],
    "Wipe time (HH:mm) 24-hour clock": "19:00"
  }
}
```

[Full Configuration Guide →](./configuration)

---

## Documentation Pages

Use the sidebar navigation or these quick links:

- **[Installation](./installation)** - Setup and prerequisites
- **[Configuration](./configuration)** - Complete settings reference
- **[Commands](./commands)** - In-game and Discord commands
- **[Auto Wipe & Voting](./wipe-scheduling)** - Schedule patterns and logic
- **[Discord Integration](./discord)** - Bot setup and configuration
- **[Changelog](./changelog)** - Version history

---

## Version Info

- **Plugin Version:** `1.7.0`
- **Config Version:** `2.0.0`
- **Last Updated:** October 6, 2025

---

## Support

Need detailed configuration examples? See the [Legacy Full Configuration Guide](./Config-Guide.html) for exhaustive tables and scenarios.

Have questions or feedback? Contact via [Codefling](https://codefling.com/plugins/map-voter-and-auto-wipe-script).
