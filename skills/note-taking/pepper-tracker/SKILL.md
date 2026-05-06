---
name: pepper-tracker
description: Use when starting, continuing, or asked about any project or task. Also before gateway/OS restarts. Pepper.md is the project source of truth — check it first, update it always.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [project-tracking, obsidian, session-continuity, pepper]
    related_skills: [obsidian]
---

# Pepper Tracker — pepper.md Source of Truth

## Overview

`pepper.md` (`/home/pepper/vault/2-Life/Pepper.md`) is Pepper's project source of truth and continuity document. It serves two audiences:

1. **Pepper (after restart):** Regain context on active projects, in-flight work, and what was happening before the gateway went down.
2. **DK (human window):** See at a glance what's being worked on, what's stalled, and what's next — without having to ask.

Every session that touches a project MUST update pepper.md. Every restart MUST produce a checkpoint update. This is non-negotiable — it's how continuity works across gateway restarts and session compaction.

## When to Use

**Always check pepper.md FIRST when:**
- DK asks "what are we working on?" or "what's the status of X?"
- A project is mentioned and you don't immediately recall the details
- Starting a new session after a gateway restart (read it as early context)
- Planning what to work on next

**Always UPDATE pepper.md when:**
- Starting a new project (add it to Active Projects)
- Making progress on an existing project (update status/next step)
- A project is completed (move to Backlog or mark done)
- A project is shelved/cancelled (update status with reason)
- **Before ANY gateway restart or OS reboot** — checkpoint current state
- Discovering something that was forgotten or dropped
- DK gives new instructions that change project direction

**Don't use for:**
- One-off questions that don't relate to a project
- Internal agent state that DK doesn't need visibility into
- Replacing the todo tool for session-scoped task lists

## File Location and Structure

**Path:** `/home/pepper/vault/2-Life/Pepper.md`

### Current Sections

| Section | Purpose |
|---------|---------|
| `## 📌 From Pepper` | Notes TO DK — updates, fixes, things DK should see |
| `## 🧱 Active Projects` | Table of current projects with status and next steps |
| `## <Project Name> — WIP` | Deep-dive section for in-flight projects with architecture, files, progress |
| `## 📋 Backlog` | Shelved, pending, or deferred items |
| `## 🤖 Agent Fleet` | Fleet status and cross-agent communication |
| `## ⏰ Automated Tasks` | Cron jobs and their health |
| `*Last Updated: ...*` | Timestamp footer — update on every edit |

### Updating Rules

1. **Read before write.** Always `read_file` the current content before editing — never write from memory or a stale snapshot.
2. **Use `patch` for surgical updates.** Change only what needs changing. Don't rewrite the whole file for a status update.
3. **Use `write_file` only for structural changes** — adding a new WIP section, restructuring tables, or when too many scattered patches would be worse than a clean rewrite.
4. **Update the timestamp** at the bottom every time you touch the file: `*Last Updated: YYYY-MM-DD HH:MM ET*`
5. **Keep the format consistent.** Match the existing markdown patterns — tables use `|`, projects use emoji status indicators.

### Status Indicators

| Emoji | Meaning |
|-------|---------|
| 🟢 | Done / Live / No action needed |
| 🟡 | In Progress / Needs revisit |
| 🔴 | Blocked / Urgent |
| ⏳ | Pending (waiting on DK or external) |
| 🏗️ | Planning phase |

## Gateway Restart Protocol

**⛔ MANDATORY — Before restarting the gateway or rebooting the OS:**

1. **Read** the current pepper.md
2. **Update** the Active Projects table with exact current state:
   - What was being worked on this session
   - What's done vs what's still in-flight
   - What needs to happen next (specific enough that a fresh session can pick it up)
3. **Add a checkpoint note** to "From Pepper" if the session produced anything noteworthy
4. **Update the timestamp**

This takes ~2 minutes and prevents context loss. Do NOT skip this.

## New Project Template

When starting a new project, add it to the Active Projects table AND create a detail section if it's non-trivial:

```markdown
| N | **Project Name** | 🟡 In Progress | Brief next step |

## Project Name — WIP

**Started:** YYYY-MM-DD
**Origin:** How/why this project exists (brief)

### What it does
One-paragraph description.

### Progress
- [x] Thing that's done
- [ ] Thing that's not done yet

### Next steps
1. Specific action item
2. Specific action item

### Notes
Any gotchas, decisions made, things to remember.
```

## Priority for Project Recall

When DK mentions a project and you need context, use this order:

1. **pepper.md** — always first, it's the source of truth
2. **session_search** — for details from recent conversations
3. **Obsidian project notes** — if pepper.md references a specific note
4. **lcm_grep** — for details earlier in the current session
5. **Ask DK** — only if all sources come up empty

If all sources come up empty, it's a NEW project — create the section in pepper.md and start tracking it.

## Common Pitfalls

1. **Writing pepper.md from memory instead of reading first.** The file may have been updated by another session or another agent. Always read first.
2. **Skipping the restart checkpoint.** "We're just doing a quick restart" — no. Two minutes now saves twenty minutes of "wait, where were we?" later.
3. **Vague next steps.** "Keep working on it" is not a next step. "Write tests for `discord_components.py` — specifically test REST path interaction routing" IS a next step. A fresh session needs to be able to pick up without asking DK what the plan was.
4. **Forgetting to update the timestamp.** DK uses it to know if the file is fresh. A stale timestamp means stale data.
5. **Letting Active Projects grow stale.** If a project hasn't been touched in weeks, either move it to Backlog with context, or explicitly confirm with DK that it's still active.
6. **Over-writing the whole file for a small change.** Use `patch` for status updates, table edits, and timestamp bumps. Only `write_file` when restructuring.

## Verification Checklist

After updating pepper.md, verify:
- [ ] Timestamp updated at the bottom
- [ ] Active Projects table reflects reality (no ghost projects, no missing ones)
- [ ] WIP sections are current (not describing work that's already done)
- [ ] Next steps are specific enough for a fresh session to act on
- [ ] No merge conflicts or partial writes (read back the file to confirm)
