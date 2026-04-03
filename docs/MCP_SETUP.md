# MCP Setup

This project exposes an MCP stdio server from:

`node src/mcp-server.mjs`

Use an absolute path to the checked-out repository when registering the server.

## Codex CLI

Add the server:

```powershell
codex mcp add rexd -- node <ABSOLUTE_PATH_TO_REPO>\src\mcp-server.mjs
```

Verify it:

```powershell
codex mcp list
codex mcp get rexd
```

Example file:

- [examples/codex-add.txt](../examples/codex-add.txt)

## Claude Code

Direct add:

```powershell
claude mcp add rexd -- node <ABSOLUTE_PATH_TO_REPO>\src\mcp-server.mjs
```

JSON add:

```powershell
claude mcp add-json rexd "{\"type\":\"stdio\",\"command\":\"node\",\"args\":[\"<ABSOLUTE_PATH_TO_REPO>\\\\src\\\\mcp-server.mjs\"],\"env\":{}}"
```

Example files:

- [examples/claude-code-add-json.txt](../examples/claude-code-add-json.txt)
- [examples/claude-code-mcp.json](../examples/claude-code-mcp.json)

## Notes

- Replace `<ABSOLUTE_PATH_TO_REPO>` with your local checkout path.
- If `node` is not in `PATH`, replace it with the full path to your Node executable.
- Use an interactive agent session when your client requires approval for MCP tool calls.

## Example prompt

After registering the server and launching the native demo target:

```text
Use the `rexd` MCP server.

Attach to the running `number_target.exe` process.
Find the displayed number in memory and change it to `100`.
Prefer using the PID or memory address shown in the window title if available.
Report the exact MCP tool calls you used and the final value you confirmed.
```
