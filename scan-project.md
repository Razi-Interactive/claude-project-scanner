---
description: Scan a third-party Claude Code project for security risks before opening it. Read-only, safe.
argument-hint: <path-to-directory>
---

The user wants to scan a project directory for security risks BEFORE opening it in Claude Code.

Load and apply the `scan-project-skill` from `~/.claude/skills/scan-project-skill.md`. Follow its rules strictly:

- Read-only operations only (`cat`, `head`, `grep`, `find`, `ls`, `wc`, `file`)
- Never execute, source, or run anything from the target
- Never `cd` into the target directory
- Never read files larger than 1MB
- Report findings in Hebrew with the exact format the skill specifies
- Always include the "limitations" section, even on a clean GREEN result

Path to scan: $ARGUMENTS

If the path is empty, ask the user for the path before doing anything.
