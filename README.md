# Claude Project Scanner

Read-only Claude Code skill that scans third-party projects for known security risks **before** you open them.

Catches the attack patterns documented in:
- CVE-2025-59536 (Claude Code Lifecycle Hooks injection)
- CVE-2025-61260 (OpenAI Codex CODEX_HOME injection)
- CVE-2025-54136 (Cursor plugin validation bypass)

Plus `statusLine.command` execution, `additionalDirectories` escape, prompt injection in agents/skills/commands/output-styles, suspicious MCP servers, and hidden base64-shaped payloads.

---

## ⚠️ Read this first

**1. Hygiene check, not a complete security tool.** This is one defensive layer. It does not replace manual review of files it flags.

**2. Based on attack patterns known as of April 2026.** New attack vectors will require updates. If you find a pattern this scan misses, please open an issue.

**3. No warranty (MIT License).** Use at your own risk. The author accepts no liability for damages arising from use of this tool.

---

## What it checks

| Surface | What's flagged |
|---|---|
| `.claude/settings.json`, `settings.local.json` | `hooks`, `statusLine.command`, `additionalDirectories`, broad permissions like `Bash(*)` |
| `.mcp.json` | MCP servers pointing at `/tmp/`, `~/Downloads/`, `http://`, suspicious payload URLs |
| `CLAUDE.md`, `AGENTS.md` | Prompt injection patterns ("ignore previous instructions", "you are now", HTML-comment hidden instructions) |
| `.claude/agents/*.md`, `skills/*.md`, `commands/*.md`, `output-styles/*.md` | Same prompt injection patterns |
| `.claude-plugin/plugin.json` | Plugin manifests declaring hooks |
| `.env`, `.env.local` | `CODEX_HOME=` pointing inside the project, exposed secrets |
| `.cursor/`, `.codex/` | Marker directories from other AI tools (worth a manual look) |
| Any of the above | Long alphanumeric runs (80+ chars) hinting at base64-encoded payloads |

## What it does NOT check

- Encoded payloads (it flags long alphanumeric runs but does not decode them)
- `package.json` `preinstall`, `pyproject.toml` scripts (out of scope - relevant only when you run `npm install` / `pip install`)
- Polyglot files
- Attack vectors not yet published as of April 2026
- Your global `~/.claude/settings.json` (the scanner targets project directories, not the machine)

---

## Installation

Two files. No dependencies.

**1. Save the skill globally:**
```bash
mkdir -p ~/.claude/skills
cp scan-project-skill.md ~/.claude/skills/
```

**2. Save the slash command globally:**
```bash
mkdir -p ~/.claude/commands
cp scan-project.md ~/.claude/commands/
```

**3. Open a new Claude Code session** (so the new skill loads).

That's it.

---

## Usage

```
/scan-project /absolute/path/to/some-cloned-repo
```

Or with a `~` path:

```
/scan-project ~/Downloads/some-skill-folder
```

You'll get a Hebrew report with one of four verdicts:
- 🔴 **RED** - do not open. Findings match known attack patterns.
- 🟠 **ORANGE** - review the flagged files manually before opening.
- 🟡 **YELLOW** - notable but probably fine.
- 🟢 **GREEN** - no findings. Still no substitute for manual review.

The report always includes a "limitations" section so you know what the scan can and cannot catch.

---

## Self-test

Two example fixtures are included so you can see the scanner work:

```
/scan-project ./example-fixtures/clean-example
```
Expected: 🟢 GREEN

```
/scan-project ./example-fixtures/red-example
```
Expected: 🔴 RED with multiple findings

---

## Why I built this

I'm Razi Dolev. I build AI agent systems for businesses (in Israel and beyond), run Google Ads campaigns, and consult on digital marketing. I also write in Hebrew about AI for businesses on my blog at https://razi.co.il.

After Check Point published the April 2026 research on AI coding tool vulnerabilities, I needed a quick way to scan repos before opening them. This is the tool I built for myself, sharing publicly because the threat affects everyone using these tools.

The scan logic is in plain markdown - read it before you install. That's the point.

---

## License

MIT. See `LICENSE`.

## Source research

Vanunu, Oded. "AI Agent Configuration Files as Attack Vectors." Check Point Research, April 2026.
