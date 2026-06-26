---
layout: default
title: Full Configuration Guide
parent: MapVoter
nav_order: 99
permalink: /MapVoter/full-guide/
description: Exhaustive reference for every MapVoter configuration option, command, and wipe-schedule scenario.
---

# MapVoter Configuration Guide

**Plugin Version**: 1.7.0  
**Config Version**: 2.0.0  
**Last Updated**: October 6, 2025

---

## Table of Contents

1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [Commands Reference](#commands-reference)
   - [In-Game Commands](#in-game-commands)
   - [Discord Commands](#discord-commands-1)
4. [Configuration Sections](#configuration-sections)
   - [Config Version](#config-version)
   - [UI Theme System](#ui-theme-system)
   - [Commands](#commands)
   - [Rewards](#rewards)
   - [Fun Kit](#fun-kit)
   - [Options](#options)
   - [RustMaps Settings](#rustmaps-settings)
   - [Discord Settings](#discord-settings)
   - [Auto Vote](#auto-vote)
   - [Auto Wipe](#auto-wipe)
   - [Plugins Data Wipe](#plugins-data-wipe)
5. [Common Scenarios](#common-scenarios)
6. [Wipe Schedule Calculator](#wipe-schedule-calculator)
7. [Voting Timeline Explained](#voting-timeline-explained)
8. [Server Identity Explained](#server-identity-explained)
9. [Troubleshooting](#troubleshooting)

---

## Overview

MapVoter is a comprehensive Rust server plugin that automates map voting, wipe scheduling, and Discord integration. This guide explains every configuration option in detail.

**Key Features:**
- 🗳️ Automated map voting system (in-game UI + Discord bot)
- 🗺️ RustMaps.com API integration for procedural and custom maps
- ⏰ Flexible wipe scheduling (weekly, bi-weekly, monthly, custom)
- 🤖 Full Discord bot integration with voting, commands, and notifications
- 🎁 Reward system for voters (ServerRewards points, Kits)
- 🧹 Automatic plugin data cleanup on wipe
- 🎉 Fun Kit system for pre-wipe events

---

## Quick Start

### Minimum Required Configuration

```json
{
  "Config Version": "2.0.0",
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

**Get Your API Key & ORG ID:**
1. Visit https://rustmaps.com/user/profile
2. Copy your API Key and Organization ID
3. Paste them into the config

---

## Commands Reference

MapVoter provides both in-game console commands and Discord bot commands for managing voting and wipe operations.

### In-Game Commands

#### Player Commands

| Command | Description | Permission Required |
|---------|-------------|--------------------|
| `/mvote` | Opens the MapVoter voting UI | None (or `mapvoter.vote` if restricted) |
| `/voteresult` | Shows current voting results and standings | None |

#### Admin Commands - Voting

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `/startvote` | `/startvote` | Starts voting after selecting which maps appear on the ballot | `/startvote` |
| `MapVoter.generate` | `MapVoter.generate <number> <seed> <duration>` | Generates map options for voting | `MapVoter.generate 5 12345 60` |
| `MapVoter.stopvoting` | `MapVoter.stopvoting` | Stops the current voting session immediately | `MapVoter.stopvoting` |

**MapVoter.generate Parameters:**
- `<number>` - Number of maps to generate (1-10 recommended)
- `<seed>` - Map seed (use 0 for random)
- `<duration>` - Voting duration in minutes

**Example**:
```
MapVoter.generate 5 0 60
```
*Generates 5 random maps with 60-minute voting duration*

---

#### Admin Commands - Map Wipe

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `MapVoter.mapwipe` | `MapVoter.mapwipe <delay> <size>` | Wipes map to procedural with specified size | `MapVoter.mapwipe 300 3500` |
| `MapVoter.mapwipe` | `MapVoter.mapwipe <delay> <custom_url>` | Wipes map to custom map from URL | `MapVoter.mapwipe 300 https://example.com/map.map` |

**Parameters:**
- `<delay>` - Delay in seconds before wipe executes
- `<size>` - Map size (1000-6000)
- `<custom_url>` - Direct URL to `.map` file

**What it does:**
- Generates new map (procedural or downloads custom)
- Updates `server.cfg` startup parameters
- Wipes map data
- Restarts server with new map

**Examples**:
```
MapVoter.mapwipe 300 3500
```
*Wipes to 3500 size procedural map after 5 minutes*

```
MapVoter.mapwipe 60 https://rustmaps.com/download/123456
```
*Wipes to custom map after 1 minute*

---

#### Admin Commands - Blueprint Wipe

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `MapVoter.bpwipe` | `MapVoter.bpwipe <delay> <size>` | Full wipe (map + blueprints) with procedural map | `MapVoter.bpwipe 300 3500` |
| `MapVoter.bpwipe` | `MapVoter.bpwipe <delay> <custom_url>` | Full wipe (map + blueprints) with custom map | `MapVoter.bpwipe 300 https://example.com/map.map` |

**Parameters:**
- `<delay>` - Delay in seconds before wipe executes
- `<size>` - Map size (1000-6000)
- `<custom_url>` - Direct URL to `.map` file

**What it does:**
- Generates new map (procedural or downloads custom)
- Deletes blueprint/player data files:
  - `*.db` (player data)
  - `*.db-journal` (database journals)
  - `*.sav` (save files)
- Updates `server.cfg` startup parameters
- Restarts server with fresh map and reset blueprints

**Examples**:
```
MapVoter.bpwipe 600 4000
```
*Full wipe to 4000 size map after 10 minutes*

```
MapVoter.bpwipe 120 https://rustmaps.com/download/789012
```
*Full wipe to custom map after 2 minutes*

---

#### Admin Commands - Wipe Management

| Command | Description | Example |
|---------|-------------|--------|
| `MapVoter.cancelwipe` | Cancels any scheduled wipe (map or BP) | `MapVoter.cancelwipe` |

---

#### Admin Commands - Server Updates

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `MapVoter.update` | `MapVoter.update <delay>` | Updates server and Oxide, then restarts | `MapVoter.update 300` |
| `MapVoter.CancelUpdate` | `MapVoter.CancelUpdate` | Cancels scheduled update | `MapVoter.CancelUpdate` |

**What MapVoter.update does:**
1. Announces update to players
2. Waits specified delay (in seconds)
3. Updates Rust server files
4. Updates Oxide framework
5. Restarts server

**Example**:
```
MapVoter.update 600
```
*Updates server after 10-minute warning*

---

#### Admin Commands - Plugin Management

| Command | Description | Example |
|---------|-------------|--------|
| `MapVoter.reload` | Reloads the plugin and configuration | `MapVoter.reload` |

---

### Discord Commands

All Discord commands use the prefix defined in your config (default: `!`).

#### Public Discord Commands

| Command | Description | Example |
|---------|-------------|--------|
| `!vote` | Shows current voting results and map options in Discord | `!vote` |

---

#### Admin Discord Commands - Voting

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `!generate` | `!generate <number> <seed> <duration>` | Generates map options for voting | `!generate 5 0 60` |
| `!stopvoting` | `!stopvoting` | Stops the current voting session | `!stopvoting` |

---

#### Admin Discord Commands - Map Wipe

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `!mapwipe` | `!mapwipe <delay> <size>` | Wipes map to procedural with specified size | `!mapwipe 300 3500` |
| `!mapwipe` | `!mapwipe <delay> <custom_url>` | Wipes map to custom map from URL | `!mapwipe 300 https://example.com/map.map` |

---

#### Admin Discord Commands - Blueprint Wipe

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `!bpwipe` | `!bpwipe <delay> <size>` | Full wipe (map + blueprints) with procedural map | `!bpwipe 300 3500` |
| `!bpwipe` | `!bpwipe <delay> <custom_url>` | Full wipe (map + blueprints) with custom map | `!bpwipe 300 https://example.com/map.map` |

---

#### Admin Discord Commands - Wipe Management

| Command | Description | Example |
|---------|-------------|--------|
| `!cancelwipe` | Cancels any scheduled wipe | `!cancelwipe` |

---

#### Admin Discord Commands - Server Updates

| Command | Syntax | Description | Example |
|---------|--------|-------------|--------|
| `!update` | `!update <delay>` | Updates server and Oxide, then restarts | `!update 300` |
| `!cancelupdate` | `!cancelupdate` | Cancels scheduled update | `!cancelupdate` |

---

### Command Permissions

#### In-Game Permissions

| Permission | Description | Default |
|------------|-------------|--------|
| `mapvoter.vote` | Required to vote (if voting is restricted) | Not required by default |
| `mapvoter.admin` | Required for all admin commands | Admin/moderator only |

**Granting Permissions:**
```
oxide.grant user <username> mapvoter.admin
oxide.grant group admin mapvoter.admin
```

#### Discord Permissions

Discord command permissions are configured in the plugin config under:
```json
"Discord Command Role Assignment (Empty = All roles can use command.)": {
  "generate": ["Admin", "Moderator"],
  "mapwipe": ["Admin"],
  "bpwipe": ["Admin"],
  "update": ["Admin"],
  "stopvoting": ["Admin", "Moderator"],
  "cancelwipe": ["Admin"],
  "cancelupdate": ["Admin"]
}
```

- Empty array `[]` = Everyone can use the command
- Add Discord role names to restrict access
- Role names are case-sensitive

---

### Common Command Workflows

#### Starting a Manual Vote

1. Generate maps: `MapVoter.generate 5 0 60`
2. Select maps for voting in the UI
3. Start vote: `/startvote`
4. Players vote using `/mvote`
5. Check results: `/voteresult` or `!vote`

#### Emergency Map Wipe

1. Announce wipe: Use Discord/in-game chat
2. Execute wipe: `MapVoter.mapwipe 300 3500`
3. Server wipes after 5 minutes
4. Players reconnect to new map

#### Scheduled Blueprint Wipe

1. Announce BP wipe: Give players warning
2. Execute BP wipe: `MapVoter.bpwipe 600 4000`
3. Server wipes map + BPs after 10 minutes
4. Fresh start for all players

#### Server Update Process

1. Check for Rust update
2. Announce: "Server updating in 10 minutes"
3. Execute: `MapVoter.update 600`
4. Server updates and restarts automatically

---

## Configuration Sections

### Config Version

```json
"Config Version": "2.0.0"
```

**Purpose**: Tracks the configuration structure version for future migrations.

⚠️ **Do not modify this value manually.** The plugin uses this to automatically migrate old configurations.

---

### UI Theme System

**New in Version 1.7.0** - Complete UI customization system for colors, spacing, typography, and effects.

```json
"Theme": {
  "Colors": {
    "Primary": "#4a90e2",
    "Secondary": "#5cb85c",
    "Success": "#6a8b38",
    "Warning": "#d08822",
    "Error": "#d85540",
    "Info": "#007acc",
    "Background": "#232323",
    "Surface": "#2a2a2a",
    "TextPrimary": "#ffffff",
    "TextSecondary": "#b0b0b0",
    "Border": "#404040",
    "Shadow": "#000000",
    "Accent": "#ff6b35"
  },
  "Spacing": {
    "Tiny": 0.002,
    "Small": 0.005,
    "Medium": 0.01,
    "Large": 0.02,
    "ExtraLarge": 0.04
  },
  "Typography": {
    "TinySize": 10,
    "SmallSize": 12,
    "BodySize": 14,
    "HeaderSize": 16,
    "TitleSize": 20
  },
  "Effects": {
    "EnableShadows": false,
    "EnableGlow": false,
    "EnableBlur": true,
    "EnableAnimations": false
  }
}
```

---

#### Color Scheme

The color scheme defines all colors used throughout the MapVoter UI. All colors use hex format (`#RRGGBB`).

| Color Property | Default | Usage | Examples |
|----------------|---------|-------|----------|
| **Primary** | `#4a90e2` (Blue) | Main action buttons, primary UI elements | Vote buttons, navigation |
| **Secondary** | `#5cb85c` (Green) | Secondary actions, alternative buttons | Map selection, filters |
| **Success** | `#6a8b38` (Green) | Success states, vote buttons, positive actions | "Vote" button, success notifications |
| **Warning** | `#d08822` (Orange) | Warning states, editor labels, caution indicators | Editor field labels, warnings |
| **Error** | `#d85540` (Red) | Error states, cancel buttons, delete actions | Close buttons, error notifications, voted state |
| **Info** | `#007acc` (Blue) | Information displays, section separators | Editor section headers, info notifications |
| **Background** | `#232323` (Dark Gray) | Main background panels | Menu backgrounds, card backgrounds |
| **Surface** | `#2a2a2a` (Lighter Gray) | Card surfaces, elevated panels | Map cards, input fields |
| **TextPrimary** | `#ffffff` (White) | Primary text, headings, important labels | Titles, button text, main content |
| **TextSecondary** | `#b0b0b0` (Light Gray) | Secondary text, descriptions, less important labels | Subtitles, hints, metadata |
| **Border** | `#404040` (Medium Gray) | Borders, dividers, outlines | Card borders, separators |
| **Shadow** | `#000000` (Black) | Drop shadows, depth effects | Card shadows (when enabled) |
| **Accent** | `#ff6b35` (Orange-Red) | Highlight colors, special indicators | Featured items, highlights |

**Color Customization Tips:**
- Use consistent saturation levels for a professional look
- Maintain good contrast between text and backgrounds (WCAG AA: 4.5:1 minimum)
- Test colors in-game at different times of day (Rust lighting affects UI visibility)
- Keep Primary/Secondary distinct from Success/Error for clear visual hierarchy

**Example Color Schemes:**

**Dark Blue Theme:**
```json
"Colors": {
  "Primary": "#2563eb",
  "Secondary": "#3b82f6",
  "Success": "#10b981",
  "Warning": "#f59e0b",
  "Error": "#ef4444",
  "Info": "#06b6d4",
  "Background": "#1e293b",
  "Surface": "#334155",
  "TextPrimary": "#f1f5f9",
  "TextSecondary": "#cbd5e1",
  "Border": "#475569",
  "Shadow": "#0f172a",
  "Accent": "#8b5cf6"
}
```

**Light Mode (Experimental):**
```json
"Colors": {
  "Primary": "#2563eb",
  "Secondary": "#16a34a",
  "Success": "#22c55e",
  "Warning": "#f97316",
  "Error": "#dc2626",
  "Info": "#0ea5e9",
  "Background": "#f8fafc",
  "Surface": "#ffffff",
  "TextPrimary": "#0f172a",
  "TextSecondary": "#64748b",
  "Border": "#cbd5e1",
  "Shadow": "#94a3b8",
  "Accent": "#a855f7"
}
```

---

#### Spacing System

Spacing values control padding, margins, and gaps throughout the UI. Values are in normalized screen coordinates (0.0 to 1.0).

| Spacing Property | Default | Pixel Equivalent (1920x1080) | Usage |
|------------------|---------|------------------------------|-------|
| **Tiny** | `0.002` | ~2px | Minimal gaps, fine adjustments |
| **Small** | `0.005` | ~5px | Small padding, tight spacing |
| **Medium** | `0.01` | ~10px | Standard padding, comfortable spacing |
| **Large** | `0.02` | ~20px | Large gaps, section separation |
| **ExtraLarge** | `0.04` | ~40px | Major sections, page margins |

**Spacing Usage Examples:**
- **Tiny**: Border thickness, small adjustments
- **Small**: Button padding, card padding
- **Medium**: Standard content margins
- **Large**: Section gaps, header spacing
- **ExtraLarge**: Page margins, major layout divisions

**Responsive Spacing:**
Spacing automatically scales with screen resolution. The pixel equivalents shown are approximate for 1920x1080 displays.

---

#### Typography System

Typography controls font sizes throughout the UI. All values are in points (pt).

| Typography Property | Default | Usage | Examples |
|---------------------|---------|-------|----------|
| **TinySize** | `10` | Very small text, metadata | Timestamps, version numbers |
| **SmallSize** | `12` | Small text, labels, secondary content | Input field labels, vote counts, descriptions |
| **BodySize** | `14` | Standard body text, buttons | Button text, content text, standard labels |
| **HeaderSize** | `16` | Section headers, subsections | Panel headers, category titles |
| **TitleSize** | `20` | Main titles, page headers | Menu title, main headings |

**Typography Best Practices:**
- Keep at least 2pt difference between adjacent sizes for clear hierarchy
- Don't go below 10pt for readability (especially at 1080p or lower)
- Test readability at your server's most common resolution
- Consider players with visual impairments - larger is generally better

**Custom Typography Examples:**

**Larger Text (Accessibility):**
```json
"Typography": {
  "TinySize": 12,
  "SmallSize": 14,
  "BodySize": 16,
  "HeaderSize": 18,
  "TitleSize": 22
}
```

**Compact UI:**
```json
"Typography": {
  "TinySize": 9,
  "SmallSize": 11,
  "BodySize": 13,
  "HeaderSize": 15,
  "TitleSize": 18
}
```

---

#### Visual Effects

Visual effects control advanced UI features like shadows, glow, blur, and animations.

| Effect Property | Default | Performance Impact | Description |
|----------------|---------|-------------------|-------------|
| **EnableShadows** | `false` | Low | Drop shadows on cards and panels for depth |
| **EnableGlow** | `false` | Medium | Glow effects on highlighted elements |
| **EnableBlur** | `true` | Medium | Background blur for better readability |
| **EnableAnimations** | `false` | Low-Medium | Smooth transitions and animations |

**Performance Considerations:**
- **Shadows**: Minimal impact, safe to enable on most servers
- **Glow**: Moderate impact, may affect performance on busy UIs
- **Blur**: Moderate impact, recommended to keep enabled for readability
- **Animations**: Low impact, mainly affects perceived performance

**Effect Recommendations by Server Type:**

**High-Performance Server (200+ FPS):**
```json
"Effects": {
  "EnableShadows": true,
  "EnableGlow": true,
  "EnableBlur": true,
  "EnableAnimations": true
}
```

**Standard Server (60-120 FPS):**
```json
"Effects": {
  "EnableShadows": true,
  "EnableGlow": false,
  "EnableBlur": true,
  "EnableAnimations": false
}
```

**Potato Server (Low-end):**
```json
"Effects": {
  "EnableShadows": false,
  "EnableGlow": false,
  "EnableBlur": false,
  "EnableAnimations": false
}
```

---

#### Complete Theme Examples

**Corporate Professional:**
```json
"Theme": {
  "Colors": {
    "Primary": "#1e40af",
    "Secondary": "#059669",
    "Success": "#10b981",
    "Warning": "#f59e0b",
    "Error": "#dc2626",
    "Info": "#0284c7",
    "Background": "#1e293b",
    "Surface": "#334155",
    "TextPrimary": "#f8fafc",
    "TextSecondary": "#cbd5e1",
    "Border": "#475569",
    "Shadow": "#0f172a",
    "Accent": "#8b5cf6"
  },
  "Spacing": {
    "Tiny": 0.002,
    "Small": 0.006,
    "Medium": 0.012,
    "Large": 0.024,
    "ExtraLarge": 0.048
  },
  "Typography": {
    "TinySize": 10,
    "SmallSize": 12,
    "BodySize": 14,
    "HeaderSize": 16,
    "TitleSize": 20
  },
  "Effects": {
    "EnableShadows": true,
    "EnableGlow": false,
    "EnableBlur": true,
    "EnableAnimations": false
  }
}
```

**Gaming Neon:**
```json
"Theme": {
  "Colors": {
    "Primary": "#ec4899",
    "Secondary": "#8b5cf6",
    "Success": "#10b981",
    "Warning": "#fbbf24",
    "Error": "#ef4444",
    "Info": "#06b6d4",
    "Background": "#0f0f0f",
    "Surface": "#1a1a1a",
    "TextPrimary": "#ffffff",
    "TextSecondary": "#a0a0a0",
    "Border": "#333333",
    "Shadow": "#000000",
    "Accent": "#f97316"
  },
  "Spacing": {
    "Tiny": 0.003,
    "Small": 0.007,
    "Medium": 0.014,
    "Large": 0.028,
    "ExtraLarge": 0.056
  },
  "Typography": {
    "TinySize": 11,
    "SmallSize": 13,
    "BodySize": 15,
    "HeaderSize": 17,
    "TitleSize": 22
  },
  "Effects": {
    "EnableShadows": true,
    "EnableGlow": true,
    "EnableBlur": true,
    "EnableAnimations": true
  }
}
```

**Minimalist Clean:**
```json
"Theme": {
  "Colors": {
    "Primary": "#2563eb",
    "Secondary": "#10b981",
    "Success": "#059669",
    "Warning": "#d97706",
    "Error": "#dc2626",
    "Info": "#0284c7",
    "Background": "#18181b",
    "Surface": "#27272a",
    "TextPrimary": "#fafafa",
    "TextSecondary": "#a1a1aa",
    "Border": "#3f3f46",
    "Shadow": "#09090b",
    "Accent": "#6366f1"
  },
  "Spacing": {
    "Tiny": 0.002,
    "Small": 0.005,
    "Medium": 0.01,
    "Large": 0.02,
    "ExtraLarge": 0.04
  },
  "Typography": {
    "TinySize": 10,
    "SmallSize": 12,
    "BodySize": 14,
    "HeaderSize": 16,
    "TitleSize": 19
  },
  "Effects": {
    "EnableShadows": false,
    "EnableGlow": false,
    "EnableBlur": true,
    "EnableAnimations": false
  }
}
```

---

#### Theme Troubleshooting

**Problem: Colors look washed out in-game**
- **Solution**: Increase color saturation, use darker backgrounds, ensure sufficient contrast

**Problem: Text is hard to read**
- **Solution**: Increase `TextPrimary` to `#ffffff`, darken `Background` color, enable `EnableBlur`

**Problem: UI feels cramped**
- **Solution**: Increase all `Spacing` values by 25-50%, increase `Typography` sizes by 1-2pt

**Problem: UI feels too large**
- **Solution**: Decrease `Spacing` values by 25%, decrease `Typography` sizes by 1-2pt

**Problem: Performance issues**
- **Solution**: Disable all `Effects`, use simpler colors without transparency

---

### Commands

```json
"Commands": {
  "Open MapVoter UI": "mvote",
  "Generate Maps": "MapVoter.generate",
  "vote result": "voteresult"
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Open MapVoter UI** | `mvote` | In-game command players use to open the voting UI |
| **Generate Maps** | `MapVoter.generate` | Admin command to generate new map options |
| **vote result** | `voteresult` | Command to view current voting results |

**Examples:**
- Player types `/mvote` in chat → Opens voting UI
- Admin types `/MapVoter.generate 5` → Generates 5 random maps
- Anyone types `/voteresult` → Shows current vote standings

---

### Rewards

```json
"Rewards": {
  "Rust rewards": true,
  "reward points": 5,
  "Kits": true,
  "Kit name": ""
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Rust rewards** | `true` | Enable ServerRewards points for voting |
| **reward points** | `5` | Number of ServerRewards points given per vote |
| **Kits** | `true` | Enable kit rewards for voting (requires Kits plugin) |
| **Kit name** | `""` | Name of the kit given to voters (leave empty to disable) |

**Requirements:**
- **ServerRewards** plugin installed and configured by admin (for points)
- **Kits** plugin installed (for kit rewards)

**When are rewards given?**
- Immediately after a player casts their vote (not when map wins)

---

### Fun Kit

```json
"Fun Kit": {
  "Fun kit enabled": false,
  "Enable Fun kit x minutes before wipe": 120,
  "Kit name": "FunKit",
  "Permission": ""
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Fun kit enabled** | `false` | Enable the fun kit system |
| **Enable Fun kit x minutes before wipe** | `120` | Minutes before wipe to enable the kit (2 hours) |
| **Kit name** | `"FunKit"` | Name of the kit to give players |
| **Permission** | `""` | Permission required to claim kit (leave empty for everyone) |

**Purpose**: Give players special items/weapons before wipe for pre-wipe PvP events.

**Example Scenario:**
- Wipe is scheduled for 19:00
- Fun kit enabled at 120 minutes before = 17:00
- Players can claim "FunKit" from 17:00 to 19:00 for pre-wipe chaos!

---

### Options

```json
"Options": {
  "Enable file Debug mode (true/false)": false,
  "Enable Console Debug mode (true/false)": false,
  "Disable UI": false
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Enable file Debug mode** | `false` | Logs detailed debug info to `oxide/logs/MapVoter_debug.txt` |
| **Enable Console Debug mode** | `false` | Prints debug messages to server console |
| **Disable UI** | `false` | Disables the in-game voting UI (Discord-only voting) |

⚠️ **Performance Warning**: Only enable debug modes when troubleshooting issues. They generate large log files.

---

### RustMaps Settings

```json
"RustMaps Settings": {
  "Map size": 3500,
  "RustMaps API key": "https://rustmaps.com/user/profile",
  "RustMaps ORG ID": "https://rustmaps.com/user/profile",
  "Select random maps from rustmaps filter id instead of generating random maps on wipe day (true/false)": false,
  "How many pages the plugin looks up per search request(every page has 30 maps": 10,
  "staging": false,
  "barren": false,
  "filter Id": "Visit https://rustmaps.com/..."
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Map size** | `3500` | Size of generated procedural maps (1000-6000) |
| **RustMaps API key** | - | Your RustMaps.com API key (get from profile) |
| **RustMaps ORG ID** | - | Your RustMaps.com Organization ID (get from profile) |
| **Select random maps from rustmaps filter id...** | `false` | Use pre-filtered maps instead of random generation |
| **How many pages...** | `10` | Number of pages to search (30 maps per page) |
| **staging** | `false` | Include staging branch maps in search |
| **barren** | `false` | Include barren maps in search |
| **filter Id** | - | RustMaps filter ID for custom map criteria |

#### Getting Your Filter ID:

1. Visit https://rustmaps.com/
2. Adjust map requirements (size, seed range, features, etc.)
3. Click the **Share** button (red box above settings)
4. Copy the string at the end of the URL

**Example:**
- URL: `https://rustmaps.com/?share=gEU5W6BUuUG5FpPlyv2nhQ`
- Filter ID: `gEU5W6BUuUG5FpPlyv2nhQ`

**Map Size Recommendations:**
- Small servers (1-50 players): 2500-3000
- Medium servers (50-150 players): 3500-4000
- Large servers (150+ players): 4500-5500

---

### Discord Settings

```json
"Discord Settings": {
  "Discord Server ID (Optional if bot only in 1 guild)": 0,
  "Enable Discord bot (true/false)": true,
  "Log to Discord (true/false)": false,
  "Discord Logs Channel Id": "",
  "Vote Channel id": "",
  "Winning Map Channel id": "",
  "Mention role on vote start and end": "@everyone",
  "Create new thread for every vote": false,
  "Wipe time (HH:mm) 24-hour clock (leave empty if you want to use the server wipe local wipe time)": "",
  "Discord Apikey": "BotToken",
  "Discord Command Prefix": "!",
  "Discord Channels": [...],
  "Discord Command Role Assignment (Empty = All roles can use command.)": {...},
  "Single Embed?": false,
  "Discord avatar url": "",
  "Discord footer": "",
  "Embed Url": "https://rustmaps.com"
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Discord Server ID** | `0` | Your Discord server (guild) ID (optional if bot is only in 1 server) |
| **Enable Discord bot** | `true` | Enable Discord integration |
| **Log to Discord** | `false` | Send plugin events to Discord log channel |
| **Discord Logs Channel Id** | `""` | Channel ID for logs (right-click channel → Copy ID) |
| **Vote Channel id** | `""` | Channel ID where vote embeds are posted during voting |
| **Winning Map Channel id** | `""` | Channel ID where the winning map is announced after voting ends |
| **Mention role on vote start and end** | `"@everyone"` | Role to ping when votes start/end (use `||@everyone||` to suppress notification) |
| **Create new thread for every vote** | `false` | Create a new thread for each voting session |
| **Wipe time (HH:mm)** | `""` | Discord announcement time (empty = use Auto Wipe time) |
| **Discord Apikey** | `"BotToken"` | Your Discord bot token |
| **Discord Command Prefix** | `"!"` | Prefix for Discord commands (`!vote`, `!generate`) |
| **Single Embed?** | `false` | Use single embed for all maps vs. separate embeds |
| **Discord avatar url** | `""` | Custom avatar URL for bot messages |
| **Discord footer** | `""` | Custom footer text for embeds |
| **Embed Url** | `"https://rustmaps.com"` | URL when clicking embed title |

#### Setting Up Discord Bot:

1. **Create Bot:**
   - Go to https://discord.com/developers/applications
   - Create New Application → Bot → Add Bot
   - Copy Bot Token → Paste into `"Discord Apikey"`

2. **Enable Intents:**
   - Bot settings → Enable "Server Members Intent" and "Message Content Intent"

3. **Invite Bot:**
   - OAuth2 → URL Generator
   - Scopes: `bot`, `applications.commands`
   - Permissions Required:
     - ✅ Read Messages/View Channels
     - ✅ Send Messages
     - ✅ Embed Links
     - ✅ Attach Files
     - ✅ Add Reactions
     - ✅ Manage Messages (for voting reactions)
     - ✅ Create Public Threads (if using thread feature)
   - Copy URL and open in browser

4. **Get Channel IDs:**
   - Discord Settings → Advanced → Enable Developer Mode
   - Right-click channel → Copy ID

#### Discord Channels Configuration:

```json
"Discord Channels": [
  {
    "Discord Channel ID": 0,
    "Commands": [
      "generate",
      "vote",
      "mapwipe",
      "bpwipe",
      "cancelwipe",
      "stopvoting",
      "update",
      "cancelupdate"
    ]
  }
]
```

**Commands:**
- `generate` - Generate new map options for voting
- `vote` - Display current voting results and standings
- `mapwipe` - Force immediate map wipe (bypasses schedule)
- `bpwipe` - Force immediate blueprint wipe
- `cancelwipe` - Cancel a scheduled wipe
- `stopvoting` - End the current vote early
- `update` - Update server to a new map/seed immediately
- `cancelupdate` - Cancel a pending map update

#### Discord Command Role Assignment:

```json
"Discord Command Role Assignment (Empty = All roles can use command.)": {
  "generate": ["Admin", "Moderator"],
  "vote": [],
  "mapwipe": ["Admin"]
}
```

- Empty array `[]` = Everyone can use the command
- Add role names to restrict access
- Role names are case-sensitive

---

### Auto Vote

```json
"Auto Vote": {
  "Voting Settings": {
    "Stop voting after (minutes)": 60,
    "Only players with permission can vote (true/false)": false
  },
  "Enable Auto Vote (true/false)": false,
  "Only Authenticated users can vote through discord": false,
  "Start voting X days before wipe": 0,
  "Vote start (HH:mm) 24-hour clock": "17:00",
  "Number of maps generated": 4
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Stop voting after (minutes)** | `60` | Minutes after vote start time (on wipe day) to close voting. Vote ends at: `Vote start time + this value` (see [Voting Timeline](#voting-timeline-explained)) |
| **Only players with permission can vote** | `false` | Require `mapvoter.vote` permission to vote |
| **Enable Auto Vote** | `false` | Automatically start votes before scheduled wipes |
| **Only Authenticated users can vote through discord** | `false` | Only verified users can vote (users who linked Discord with Steam account) |
| **Start voting X days before wipe** | `0` | Days before wipe to start voting (0 = same day) |
| **Vote start (HH:mm) 24-hour clock** | `"17:00"` | Time to start voting (5:00 PM) |
| **Number of maps generated** | `4` | How many map options to generate for voting |

---

### Auto Wipe

```json
"Auto Wipe": {
  "Custom Map": {
    "Enable custom map (true/false)": false,
    "Custom map URL": ""
  },
  "Generate Custom Map": {
    "Generate custom map instead of procedural on vote start (true/false)": false,
    "Saved custom config name (RustMaps)": "CustomMapConfigName"
  },
  "Map Wipe schedule": [7, 14, 21, 28],
  "BP Wipe schedule": [0],
  "Server identity": "server",
  "Enable Auto Wipe (true/false)": true,
  "Wipe BPs at forced wipe day": true,
  "Forced Wipe time (HH:mm) 24-hour clock": "19:00",
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Enable custom map** | `false` | Use a specific custom map URL instead of voting |
| **Custom map URL** | `""` | Direct URL to `.map` file (must end with `.map` extension) |
| **Generate custom map instead of procedural...** | `false` | Generate custom maps (RustEdit style) for voting |
| **Saved custom config name** | `"CustomMapConfigName"` | RustMaps saved configuration name |
| **Map Wipe schedule** | `[7, 14, 21, 28]` | Days after forced wipe to wipe map (see [Calculator](#wipe-schedule-calculator)) |
| **BP Wipe schedule** | `[0]` | Days after forced wipe to wipe BPs (0 = forced wipe only) |
| **Server identity** | `"server"` | Your server's identity folder name (where server.cfg is located) - see [Server Identity Guide](#server-identity-explained) |
| **Enable Auto Wipe** | `true` | Enable automatic wipe system |
| **Wipe BPs at forced wipe day** | `true` | Wipe blueprints on forced wipe day |
| **Forced Wipe time (HH:mm)** | `"19:00"` | Time to wipe on forced wipe day (7:00 PM) |
| **Wipe time (HH:mm)** | `"19:00"` | Time to wipe on regular wipe days (7:00 PM) |

---

### Plugins Data Wipe

```json
"Plugins Data Wipe": {
  "Enable plugins data wipe on forced wipe day": false,
  "Enable plugins data wipe on map wipe day": false,
  "File names to be deleted on forced wipe day": ["exampleplugin"],
  "File names to be deleted on map wipe day": ["exampleplugin"]
}
```

| Option | Default | Description |
|--------|---------|-------------|
| **Enable plugins data wipe on forced wipe day** | `false` | Delete specified plugin data files on forced wipe (first Thursday) |
| **Enable plugins data wipe on map wipe day** | `false` | Delete specified plugin data files on regular map wipes |
| **File names to be deleted on forced wipe day** | `["exampleplugin"]` | Array of plugin data file names to delete on forced wipe |
| **File names to be deleted on map wipe day** | `["exampleplugin"]` | Array of plugin data file names to delete on map wipe |

**How It Works:**
- Plugin data files are stored in `oxide/data/` folder
- File names should match the exact plugin data file name (without `.json` extension)
- Multiple files can be specified in the array

**Examples:**

**Reset player stats on forced wipe only:**
```json
"Plugins Data Wipe": {
  "Enable plugins data wipe on forced wipe day": true,
  "Enable plugins data wipe on map wipe day": false,
  "File names to be deleted on forced wipe day": [
    "PlayerRanks",
    "PlaytimeTracker",
    "PlayerDatabase"
  ],
  "File names to be deleted on map wipe day": []
}
```

**Clear economy plugins on every map wipe:**
```json
"Plugins Data Wipe": {
  "Enable plugins data wipe on forced wipe day": true,
  "Enable plugins data wipe on map wipe day": true,
  "File names to be deleted on forced wipe day": [
    "Economics",
    "ServerRewards"
  ],
  "File names to be deleted on map wipe day": [
    "Economics",
    "ServerRewards"
  ]
}
```

**Different wipes for different schedules:**
```json
"Plugins Data Wipe": {
  "Enable plugins data wipe on forced wipe day": true,
  "Enable plugins data wipe on map wipe day": true,
  "File names to be deleted on forced wipe day": [
    "PlayerRanks",
    "Economics",
    "ServerRewards",
    "Clans"
  ],
  "File names to be deleted on map wipe day": [
    "RaidableBases",
    "DynamicPVP"
  ]
}
```

**Common Plugin Data Files to Wipe:**
- `PlayerRanks` - Player ranking/statistics
- `Economics` - Economy plugin balances
- `ServerRewards` - Reward points
- `Clans` - Clan data
- `PlaytimeTracker` - Playtime tracking
- `RaidableBases` - Raidable bases event data
- `DynamicPVP` - PVP zone data
- `ZoneManager` - Zone data
- `LootDefender` - Loot protection data

---

## Common Scenarios

### Weekly Wipes (Map Only, BP at Forced Wipe)

**Scenario**: Wipe the map every Thursday, wipe BPs only on forced wipe (first Thursday of month)

```json
"Auto Wipe": {
  "Map Wipe schedule": [7],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

**Result**:
- **First Thursday of Month**: Map + BP wipe at 19:00
- **Other Thursdays**: Map wipe only at 19:00

---

### Bi-Weekly Wipes

**Scenario**: Wipe every 2 weeks (twice per month)

```json
"Auto Wipe": {
  "Map Wipe schedule": [14],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

**Result**:
- **First Thursday**: Forced wipe + BP wipe at 19:00
- **~Two Weeks Later**: Map wipe only at 19:00

---

### Monthly Wipes (Forced Wipe Only)

**Scenario**: Wipe only on forced wipe day (first Thursday)

```json
"Auto Wipe": {
  "Map Wipe schedule": [],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

---

### Custom Schedule (Wipe on Specific Days)

**Scenario**: Wipe 3 times per month (every 10 days approximately)

```json
"Auto Wipe": {
  "Map Wipe schedule": [10, 20],
  "BP Wipe schedule": [0],
  "Wipe BPs at forced wipe day": true,
  "Wipe time (HH:mm) 24-hour clock": "19:00"
}
```

---

### Voting 1 Day Before Wipe

**Scenario**: Start voting 24 hours before wipe, close voting 1 hour before wipe

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

**Timeline** (assuming wipe at 19:00):
- **Day Before Wipe, 17:00**: Voting starts
- **Day Before Wipe, 17:00 - 23:59**: Players can vote
- **Wipe Day, 00:00 - 17:59**: Players can vote
- **Wipe Day, 18:00**: Voting ends (17:00 + 60 minutes)
- **Wipe Day, 19:00**: Server wipes to winning map

**Note**: Vote start time (17:00) is 2 hours before wipe time (19:00) - this is required!

---

## Wipe Schedule Calculator

### Understanding Forced Wipe

**Forced Wipe** occurs on the **first Thursday of every month** at the time specified by Facepunch Studios (usually around 2:00 PM EST / 19:00 UTC).

### How Schedules Work

The `"Map Wipe schedule"` and `"BP Wipe schedule"` arrays define wipe days **relative to the forced wipe date**.

**Format**: `[days_after_forced_wipe, days_after_forced_wipe, ...]`

### Examples

#### Example 1: Weekly Wipes

```json
"Map Wipe schedule": [7, 14, 21, 28]
```

**Calculation** (assuming forced wipe on January 3rd):
- **January 3 (Forced Wipe)**: Map + BP wipe
- **January 10** (3 + 7 days): Map wipe
- **January 17** (3 + 14 days): Map wipe
- **January 24** (3 + 21 days): Map wipe
- **January 31** (3 + 28 days): Map wipe
- **February 7 (Next Forced Wipe)**: Map + BP wipe (cycle repeats)

**Result**: Wipe every Thursday (weekly)

#### Example 2: Bi-Weekly Wipes

```json
"Map Wipe schedule": [14, 28]
```

**Calculation** (assuming forced wipe on January 3rd):
- **January 3 (Forced Wipe)**: Map + BP wipe
- **January 17** (3 + 14 days): Map wipe
- **January 31** (3 + 28 days): Map wipe
- **February 7 (Next Forced Wipe)**: Map + BP wipe

**Result**: Wipe every 2 weeks

#### Example 3: BP Wipe Every 2 Months

```json
"Map Wipe schedule": [7, 14, 21, 28],
"BP Wipe schedule": [0, 35]
```

**Calculation**:
- **Month 1, Day 0**: Map + BP wipe (forced)
- **Month 1, Day 7-28**: Map wipes only
- **Month 2, Day 0**: Map wipe only (no BP wipe)
- **Month 2, Day 7-28**: Map wipes only
- **Month 2, Day 35** (5 weeks after last BP wipe): BP wipe
- Cycle continues...

**Note**: `0` in BP Wipe schedule means "wipe on forced wipe day"

### Visual Calendar Example

**Configuration:**
```json
"Map Wipe schedule": [7, 14, 21, 28],
"BP Wipe schedule": [0]
```

**January 2025 Calendar:**
```
Sun Mon Tue Wed Thu Fri Sat
              1   2   3   4
                          ↑
                      Forced Wipe
                      Map + BP

 5   6   7   8   9  10  11
                      ↑
                    Map Only
                    (7 days)

12  13  14  15  16  17  18
                      ↑
                    Map Only
                    (14 days)

19  20  21  22  23  24  25
                      ↑
                    Map Only
                    (21 days)

26  27  28  29  30  31
                      ↑
                    Map Only
                    (28 days)
```

**February 2025:**
```
Sun Mon Tue Wed Thu Fri Sat
                          1
 2   3   4   5   6   7   8
              ↑
          Forced Wipe
          Map + BP
```

---

### Interactive Wipe Schedule Picker

> Tip: pick your wipe days (e.g. `[7, 14, 21, 28]` for weekly) and set them in `"Map Wipe schedule"`. See [Auto Wipe](./wipe-scheduling) for live examples.

---

## Voting Timeline Explained

### Important: How Voting Timing Works

⚠️ **Critical Rule**: Vote start time MUST be **at least 2 hours before wipe time**!

**How Voting Ends:**
Voting stops on wipe day at: `Vote start time + Stop voting after (minutes)`

**NOT** relative to wipe time, but relative to the vote start time on wipe day.

---

### Example Scenario 1: Voting 1 Day Before Wipe

**Configuration:**
```json
"Auto Vote": {
  "Start voting X days before wipe": 1,
  "Vote start (HH:mm) 24-hour clock": "17:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 60
  }
}
```

**Wipe Schedule:**
- Wipe Day: Thursday
- Wipe Time: 19:00

**Timeline:**

| Day | Time | Event |
|-----|------|-------|
| **Wednesday** | 17:00 | ✅ **Voting Starts** |
| Wednesday | 17:00 - 23:59 | 🗳️ Players can vote |
| **Thursday** | 00:00 - 17:59 | 🗳️ Players can vote |
| **Thursday** | 18:00 | ⛔ **Voting Ends** (17:00 + 60 minutes) |
| **Thursday** | 19:00 | ⏰ **Server Wipes** to winning map |

**Total Voting Time**: 25 hours (Wed 17:00 → Thu 18:00)

---

### Example Scenario 2: Same Day Voting

**Configuration:**
```json
"Auto Vote": {
  "Start voting X days before wipe": 0,
  "Vote start (HH:mm) 24-hour clock": "10:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 60
  }
}
```

**Wipe Schedule:**
- Wipe Day: Thursday
- Wipe Time: 19:00

**Timeline:**

| Day | Time | Event |
|-----|------|-------|
| **Thursday** | 10:00 | ✅ **Voting Starts** |
| **Thursday** | 10:00 - 10:59 | 🗳️ Players can vote |
| **Thursday** | 11:00 | ⛔ **Voting Ends** (10:00 + 60 minutes) |
| **Thursday** | 19:00 | ⏰ **Server Wipes** to winning map |

**Total Voting Time**: 1 hour (Thu 10:00 → Thu 11:00)

---

### Key Rules & Best Practices

#### ✅ Required Rules:

1. **Vote start time MUST be at least 2 hours before wipe time**
   - ✅ Wipe at 19:00 → Vote start at 17:00 or earlier (OK)
   - ❌ Wipe at 19:00 → Vote start at 18:00 (TOO LATE!)

2. **Voting ends at**: `Vote start time + Stop voting after (minutes)`
   - Example: Vote starts 17:00, stop after 60 min → Ends at 18:00
   - Example: Vote starts 10:00, stop after 120 min → Ends at 12:00

#### 💡 Recommended Configuration:

**Stop voting 1 hour after vote start time on wipe day:**
```json
"Stop voting after (minutes)": 60
```

This gives players a 1-hour window on wipe day to cast their final votes.

---

### Common Configurations:

#### Recommended: 1 Hour Voting Window on Wipe Day
```json
"Auto Vote": {
  "Start voting X days before wipe": 1,
  "Vote start (HH:mm) 24-hour clock": "17:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 60
  }
}
```
**Result**: 
- Voting starts: Day before wipe at 17:00
- Voting ends: Wipe day at 18:00 (17:00 + 60 min)
- Wipe happens: Wipe day at 19:00

---

#### Extended: 2 Hour Voting Window on Wipe Day
```json
"Auto Vote": {
  "Start voting X days before wipe": 1,
  "Vote start (HH:mm) 24-hour clock": "16:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 120
  }
}
```
**Result**:
- Voting starts: Day before wipe at 16:00
- Voting ends: Wipe day at 18:00 (16:00 + 120 min)
- Wipe happens: Wipe day at 19:00

---

#### Short Notice: 3 Hours Total Voting
```json
"Auto Vote": {
  "Start voting X days before wipe": 0,
  "Vote start (HH:mm) 24-hour clock": "15:00",
  "Voting Settings": {
    "Stop voting after (minutes)": 180
  }
}
```
**Result**:
- Voting starts: Wipe day at 15:00
- Voting ends: Wipe day at 18:00 (15:00 + 180 min)
- Wipe happens: Wipe day at 19:00

---

### ⚠️ Common Mistakes to Avoid

#### ❌ WRONG: Vote Start Too Close to Wipe
```json
"Vote start (HH:mm) 24-hour clock": "18:00"  // Wipe at 19:00
```
**Problem**: Less than 2 hours before wipe - will cause errors!

#### ❌ WRONG: Voting Ends After Wipe Time
```json
"Vote start (HH:mm) 24-hour clock": "17:00"
"Stop voting after (minutes)": 180  // Ends at 20:00
```
**Problem**: Wipe happens at 19:00, but voting doesn't end until 20:00!

#### ✅ CORRECT: Proper Configuration
```json
"Vote start (HH:mm) 24-hour clock": "17:00"
"Stop voting after (minutes)": 60  // Ends at 18:00
```
**Result**: Voting ends 1 hour before wipe - perfect!

---

## Server Identity Explained

The `"Server identity"` setting tells the plugin where your server's configuration files are located.

### What is Server Identity?

Server identity is a folder name in your Rust server directory that contains server-specific files:
```
rust_server/
├── server/
│   ├── your_identity_name/     ← This is your server identity
│   │   ├── cfg/
│   │   │   └── server.cfg      ← Your server configuration
│   │   ├── storage/
│   │   └── saves/
```

### Default Identity

By default, Rust uses `server` as the identity name, but you can customize it.

### How to Find Your Server Identity

**Method 1: Check Startup Command**
Look at your server startup script/batch file for the `-identity` parameter:
```bash
RustDedicated.exe -batchmode -identity "my_server_name"
                                        ^^^^^^^^^^^^^^^^
                                        This is your identity
```

**Method 2: Check Server Folder**
Navigate to your Rust server directory and look in the `server/` folder. The folder name inside is your identity.

### Configuration Examples

**Default identity:**
```json
"Server identity": "server"
```

**Custom identity:**
```json
"Server identity": "my_rust_server"
```

**Multiple servers on same machine:**
```json
// Server 1
"Server identity": "main_server"

// Server 2
"Server identity": "test_server"
```

### Why Is This Important?

The plugin needs to know the identity to:
- ✅ Load the correct map after wipe
- ✅ Write to the correct save folder
- ✅ Apply wipes to the correct server instance

⚠️ **Important**: If this is incorrect, wipes will fail or affect the wrong server!

### More Information

For detailed information about server identity and configuration:
- **Official Rust Wiki**: https://wiki.facepunch.com/rust/Creating-a-server
- **Server.cfg Setup**: Place your configuration file at: `rust/server/your_identity/cfg/server.cfg`

**Example server.cfg:**
```
server.hostname "My Awesome Rust Server"
server.description "Weekly wipes with MapVoter!"
server.url "https://myserver.com"
server.headerimage "https://myserver.com/banner.jpg"
server.worldsize 3500
server.seed 12345
server.maxplayers 100
```

**Note**: Variables in server.cfg don't use `+` or `-` prefixes (unlike command line).

---

## Troubleshooting

### Plugin Not Loading

**Symptoms**: No errors, plugin doesn't appear in `oxide.plugins`

**Solutions**:
1. Check Oxide is installed: `oxide.version` in console
2. Verify file is in `oxide/plugins/MapVoter.cs`
3. Check server console for compilation errors
4. Ensure all dependencies installed (see below)

### Required Dependencies

This plugin requires:
- **Oxide.Ext.Discord** - Discord extension for Oxide
- **Newtonsoft.Json** - JSON serialization (usually included)

**Optional Plugins:**
- **ServerRewards** - For reward points
- **Kits** - For kit rewards
- **ImageLibrary** - For custom UI images

### RustMaps API Not Working

**Symptoms**: "API Key invalid" or "No maps found"

**Solutions**:
1. Verify API key is correct (https://rustmaps.com/user/profile)
2. Check Organization ID matches your account
3. Ensure you have RustMaps subscription (required for API access)
4. Test API key directly: `curl -H "Authorization: Bearer YOUR_KEY" https://api.rustmaps.com/v1/maps`

### Discord Bot Not Responding

**Symptoms**: Bot online but doesn't respond to commands

**Solutions**:
1. Check bot token is correct
2. Verify bot has permissions:
   - Read Messages
   - Send Messages
   - Embed Links
   - Add Reactions
   - Manage Messages (for voting reactions)
3. Check channel IDs are correct (Developer Mode → Right-click → Copy ID)
4. Verify command prefix matches (`!vote` if prefix is `!`)
5. Check role restrictions in `"Discord Command Role Assignment"`

### Voting Not Starting Automatically

**Symptoms**: Manual voting works but auto-vote doesn't trigger

**Solutions**:
1. Verify `"Enable Auto Vote (true/false)": true`
2. Check `"Enable Auto Wipe (true/false)": true` (auto-vote requires auto-wipe)
3. Verify wipe schedule is configured correctly
4. Check server time matches your expectations: `oxide.time` in console
5. Enable debug mode and check logs: `"Enable Console Debug mode (true/false)": true`

### Wipes Not Happening

**Symptoms**: Wipe time passes but server doesn't wipe

**Solutions**:
1. Check `"Enable Auto Wipe (true/false)": true`
2. Verify `"Map Wipe schedule"` is not empty
3. Check server time zone vs. wipe time setting
4. Review console for wipe-related messages
5. Ensure plugin has write permissions to server directory

### Players Can't Vote

**Symptoms**: Players get "No permission" message

**Solutions**:
1. Check `"Only players with permission can vote (true/false)": false`
2. If true, grant permission: `oxide.grant user <username> mapvoter.vote`
3. Or grant to group: `oxide.grant group default mapvoter.vote`

### Custom Map Not Loading

**Symptoms**: Server wipes to procedural map instead of voted custom map

**Solutions**:
1. Verify custom map URL is valid and accessible
2. Check `.map` file format is compatible with current Rust version
3. Ensure file size is reasonable (< 100MB recommended)
4. Check server console for download errors

---

## Support & Resources

- **RustMaps.com**: https://rustmaps.com/
- **Oxide Documentation**: https://umod.org/
- **Discord Extension**: https://github.com/dassjosh/Oxide.Ext.Discord

---

## Changelog

### Version 1.7.0 - UI Refactoring Update
- ✨ **NEW**: Complete UI Theme System for full customization
  - 13 customizable colors (Primary, Secondary, Success, Warning, Error, etc.)
  - 5 spacing values (Tiny, Small, Medium, Large, ExtraLarge)
  - 5 typography sizes (TinySize through TitleSize)
  - 4 visual effects toggles (Shadows, Glow, Blur, Animations)
- 🎨 Refactored all UI components to use theme system
- 🎨 Added MVoterComponents library with 5 reusable components
- 🎨 Implemented notification system with color-coded types
- 🎨 Renamed UI constants to MVoter prefix to avoid plugin conflicts
- 📝 Added comprehensive theme configuration documentation
- ⚡ Improved UI code maintainability and consistency
- 🐛 Fixed compilation errors in notification system

### Version 2.0.0
- ✨ Reorganized configuration structure
- ✨ Added configuration versioning
- ✨ Separated RustMaps settings from general options
- ✨ Enhanced Discord settings with more options
- ✨ Improved voting settings organization

---

**Last Updated**: October 6, 2025  
**Author**: Kaysharp  
**Plugin Version**: 1.7.0
