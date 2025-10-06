---
layout: default
title: Commands
nav_order: 4
---
# Commands Reference

## Player Chat Commands
| Command | Description |
|---------|-------------|
| `/mvote` | Open voting UI |
| `/voteresult` | Show current voting results |

## Admin Commands (Console / Chat)
| Command | Syntax | Purpose |
|---------|--------|---------|
| `MapVoter.generate` | `MapVoter.generate <number> <seed> <duration>` | Prepare map options |
| `MapVoter.stopvoting` | `MapVoter.stopvoting` | Stop current vote |
| `MapVoter.mapwipe` | `MapVoter.mapwipe <delay> <size|custom_url>` | Schedule map wipe |
| `MapVoter.bpwipe` | `MapVoter.bpwipe <delay> <size|custom_url>` | Schedule full (map+BP) wipe |
| `MapVoter.cancelwipe` | `MapVoter.cancelwipe` | Cancel pending wipe |
| `MapVoter.update` | `MapVoter.update <delay>` | Update server & Oxide |
| `MapVoter.CancelUpdate` | `MapVoter.CancelUpdate` | Cancel server update |
| `MapVoter.reload` | `MapVoter.reload` | Reload plugin/config |

## Discord Commands
Prefix: configurable (default `!`).

| Command | Example | Description |
|---------|---------|-------------|
| `!vote` | `!vote` | Show voting status |
| `!generate` | `!generate 5 0 60` | Generate map options |
| `!stopvoting` | `!stopvoting` | Stop vote |
| `!mapwipe` | `!mapwipe 300 3500` | Schedule map wipe |
| `!bpwipe` | `!bpwipe 600 4000` | Schedule full wipe |
| `!cancelwipe` | `!cancelwipe` | Cancel wipe |
| `!update` | `!update 300` | Update server |
| `!cancelupdate` | `!cancelupdate` | Cancel update |

## Permissions
| Permission | Grants |
|-----------|--------|
| `mapvoter.vote` | Ability to vote when restriction enabled |
| `mapvoter.admin` | All admin commands |

Grant examples:
```
oxide.grant group admin mapvoter.admin
oxide.grant group default mapvoter.vote
```

## Typical Workflows
### Start a Manual Vote
1. `MapVoter.generate 5 0 60`
2. Select maps in UI
3. `/startvote` (if command used) or auto sequence
4. Players vote
5. Monitor with `/voteresult` or `!vote`

### Emergency Map Wipe
```
MapVoter.mapwipe 300 3500
```
Wipes after 5 minutes.

---
More detail (parameters, scenarios) coming soon in expanded migration.
