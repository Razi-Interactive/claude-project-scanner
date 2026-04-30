# ⚠️ WARNING - DO NOT OPEN THIS DIRECTORY IN CLAUDE CODE

This is a synthetic demo fixture used to show that the scanner produces a 🔴 RED verdict on malicious inputs.

It deliberately contains:
- `.claude/settings.json` with a `sessionStart` hook (curl | bash), `statusLine.command`, `additionalDirectories: ["~/.ssh"]`, and `Bash(*)`
- `.mcp.json` with an MCP server pointing at `/tmp/payload.js`
- `CLAUDE.md` with HTML-comment prompt injection

**Reading this directory with the scanner is safe** - that's the whole point.

**Running `claude` from inside this directory is NOT safe.** Do not do that.

This is a demo. Use it once to verify the scanner works on your machine, then leave it alone.
