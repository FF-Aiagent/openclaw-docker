---
name: self-improvement-proactive
description: Structured self-improvement, proactive reflection, and durable memory hygiene for OpenClaw. Use when: (1) a task fails, the user corrects the agent, or a better approach is discovered, (2) the agent is about to perform a complex or repeated task and should review prior learnings first, (3) recent sessions produced lessons worth preserving in workspace memory files, or (4) the user asks to improve the agent’s future behavior, memory habits, review process, or self-reflection workflow.
---

# Self-Improvement Proactive

A hardened OpenClaw-native version of self-improvement / proactive-agent patterns.

Goal: improve future performance **without becoming noisy, self-absorbed, or speculative**.

This skill is for:
- capturing useful lessons after mistakes or corrections,
- checking for relevant past learnings before major work,
- promoting durable lessons into the right workspace files,
- keeping reflection lightweight and grounded in actual events.

This skill is **not** for:
- constant self-narration,
- logging every tiny action,
- writing vague “be better next time” notes,
- inventing autonomous goals or self-directed projects.

## Core Rules

1. **Reflection must be evidence-based.** Only log something when a concrete trigger occurred.
2. **Prefer small, durable notes over long diaries.**
3. **Write to the right place.** Not everything belongs in long-term memory.
4. **Do not log secrets.** Never store tokens, full credentials, private keys, or raw sensitive configs.
5. **Do not interrupt the user just to self-reflect.** Improve quietly unless the user asked for a report.
6. **Review before repeating expensive mistakes.** For complex/repeated tasks, check prior learnings first.

## Quick Reference

| Situation | Action |
|---|---|
| User corrects you | Append a concise note to `memory/YYYY-MM-DD.md` |
| Command/tool/integration fails in a reusable way | Append a short error lesson to `memory/YYYY-MM-DD.md` |
| New durable preference or policy appears | Update `USER.md`, `TOOLS.md`, `AGENTS.md`, `SOUL.md`, or `MEMORY.md` as appropriate |
| About to do a complex or repeated task | Read relevant memory / docs first |
| A lesson is only temporary or situational | Keep it in daily memory, do not promote |
| A lesson changes long-term behavior | Promote to a durable file |
| User asks to improve your behavior/system | Review recent notes, propose a concrete update, then edit the right file |

## Reflection Triggers

Reflect only when at least one of these is true:

- The user says you were wrong, incomplete, unsafe, or inefficient.
- A tool call, command, API, or workflow failed in a non-trivial way.
- You discovered a better repeatable approach.
- A repeated pain point shows up across tasks.
- A durable user preference, boundary, or style choice becomes clear.
- You are starting a major task where prior mistakes are likely relevant.

If none of these happened, do not force reflection.

## Pre-Task Review Workflow

Before major or error-prone work:

1. Identify the task category.
   - config/security
   - coding/refactoring
   - external integration
   - memory / personalization
   - messaging / channel behavior
2. Read only the most relevant sources.
   - `memory/YYYY-MM-DD.md` for recent lessons
   - `MEMORY.md` for durable facts (main/private sessions only)
   - `TOOLS.md` for local setup gotchas
   - `AGENTS.md` for workflow rules
   - `SOUL.md` for behavioral constraints
3. Apply the lesson quietly.
4. Only mention the review if it materially affects the plan.

## Post-Task Capture Workflow

After a meaningful event:

1. Ask: **Will future-you benefit from this?**
2. If no, skip logging.
3. If yes, write a compact note with:
   - what happened,
   - why it mattered,
   - what to do next time.
4. Put it in the correct file.

### Default capture target

Use `memory/YYYY-MM-DD.md` for first capture.

Recommended format:

```markdown
- Learned: <short statement>
  - Context: <where/when it came up>
  - Next time: <what to do differently>
```

Keep it short. One useful bullet beats a page of reflection.

## Promotion Rules

Promote only when a lesson is durable.

### Promote to `USER.md`
Use for user-specific preferences:
- language preference
- naming preference
- preferred style/verbosity
- recurring dislikes/likes

### Promote to `TOOLS.md`
Use for environment-specific operational facts:
- local service quirks
- hostnames / aliases / integration gotchas
- channel-specific setup details
- commands that are correct for this machine/setup

### Promote to `AGENTS.md`
Use for workflow/process improvements:
- better delegation rules
- reminders about file organization
- operational habits that future sessions should follow

### Promote to `SOUL.md`
Use for deep behavioral changes:
- communication style
- boundaries
- principles about how to behave

Only change `SOUL.md` when the change is genuinely identity-level. Tell the user if you change it.

### Promote to `MEMORY.md`
Use only in main/private sessions for durable personal context:
- lasting preferences
- important decisions
- ongoing projects / commitments
- long-term context worth remembering

Do **not** dump transient troubleshooting notes into `MEMORY.md`.

## Proactive Behavior Rules

Proactive is good when it is:
- grounded in real context,
- lightweight,
- clearly helpful,
- respectful of quiet time.

Proactive is bad when it is:
- attention-seeking,
- repetitive,
- speculative,
- unrelated to the user’s current goals.

### Good proactive actions
- reviewing recent memory before a major task,
- cleaning up a repeated workflow rule,
- suggesting a concrete fix after a repeated failure,
- updating the right workspace file after explicit or obvious learning.

### Bad proactive actions
- announcing self-improvement for its own sake,
- creating elaborate self-tracking systems without need,
- logging every command failure regardless of importance,
- modifying multiple identity/workflow files for weak reasons.

## OpenClaw-Native Practices

For this workspace, prefer these storage layers:

1. `memory/YYYY-MM-DD.md` → raw session learnings
2. `MEMORY.md` → curated long-term memory (private/main only)
3. `USER.md` → user preferences and identity notes
4. `TOOLS.md` → local environment facts
5. `AGENTS.md` → operating workflow
6. `SOUL.md` → persona / principles

If a lesson can be handled by updating one of these files directly, do that instead of creating a parallel logging system.

## What to Avoid from Generic Self-Improvement Skills

Do **not** blindly import patterns from Claude/Codex skills such as:
- always-on hook spam,
- separate logging systems that duplicate workspace memory,
- generic reminders every session,
- references to unsupported tooling or wrong commands,
- speculative self-criticism detached from user value.

When adapting external skills, keep only the parts that fit OpenClaw’s real workspace model.

## Minimal Review Checklist

Use this checklist mentally after important work:

- Was there a correction, failure, or better approach?
- Is it likely to recur?
- Does future-you need a note?
- What is the smallest durable place to store it?
- Is any part sensitive and needing redaction?

If all answers are weak, skip logging.

## Suggested Local Structure

If you want a dedicated scratch area, you may create:

```text
.workspace/.learnings/
```

But in this workspace, default to the built-in memory files unless the user explicitly wants a separate learning log.

## If the user asks to improve this skill

When iterating on this skill:
1. Prefer making the trigger description sharper.
2. Remove duplicate or platform-mismatched guidance.
3. Keep the body lean; move examples to references only if necessary.
4. Bias toward fewer writes, better judgment, and stronger promotion rules.
