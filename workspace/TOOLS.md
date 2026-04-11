# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Docker Path Mapping

OpenClaw is running inside Docker for this setup.

- **Host base path (macOS):** `/Users/sf/projects/AI_code/openclaw/docker_openclaw/data/openclaw/.openclaw`
- **Container base path:** `/home/node/.openclaw`

Workspace mapping examples:

- Host `.../.openclaw/workspace` ↔ Container `/home/node/.openclaw/workspace`
- Host `.../.openclaw/workspace-dev` ↔ Container `/home/node/.openclaw/workspace-dev`
- Host `.../.openclaw/workspace-content` ↔ Container `/home/node/.openclaw/workspace-content`
- Host `.../.openclaw/workspace-ops` ↔ Container `/home/node/.openclaw/workspace-ops`
- Host `.../.openclaw/workspace-law` ↔ Container `/home/node/.openclaw/workspace-law`
- Host `.../.openclaw/workspace-finance` ↔ Container `/home/node/.openclaw/workspace-finance`

Rule of thumb:

- When editing `~/.openclaw/openclaw.json` inside the running OpenClaw environment, always use **container-visible paths** like `/home/node/.openclaw/...`
- Do **not** use host macOS paths like `/Users/sf/...` for `agents.list[].workspace`

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.
