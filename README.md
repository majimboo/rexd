# REXD

`rexd` is a local Frida-based MCP server for agent-driven reverse engineering.

It is designed for workflows like:

`agent -> rexd -> Frida -> target process`

The project exposes a small, typed tool surface for attaching to processes, reading and writing memory, scanning ranges, resolving symbols, and installing runtime hooks.

## What it includes

- a raw JSON bridge over `stdin` / `stdout`
- an MCP stdio server for Codex, Claude Code, and other MCP clients
- a native Windows demo target for visible memory-edit testing
- example scripts for hook and memory workflows

## Current tool surface

- `ping`
- `attach`
- `spawn`
- `resume`
- `detach`
- `listSessions`
- `listModules`
- `enumerateRanges`
- `enumerateExports`
- `enumerateSymbols`
- `readMemory`
- `writeMemory`
- `readUtf8`
- `scanMemory`
- `getSymbolDetails`
- `resolveTarget`
- `getBacktrace`
- `startHook`
- `listHooks`
- `removeHook`
- `drainHookEvents`

## Requirements

- Node.js 20+
- Windows for the included native demo target
- Frida-compatible target environment

## Install

Clone the repo and install dependencies:

```powershell
npm install
```

Start the MCP server:

```powershell
npm run start:mcp
```

## Register with Codex or Claude

Register `rexd` with Codex:

```powershell
codex mcp add rexd -- node <ABSOLUTE_PATH_TO_REPO>\src\mcp-server.mjs
```

Register `rexd` with Claude Code:

```powershell
claude mcp add rexd -- node <ABSOLUTE_PATH_TO_REPO>\src\mcp-server.mjs
```

If `node` is not on `PATH`, use the full path to `node.exe`.

More setup examples are in [docs/MCP_SETUP.md](docs/MCP_SETUP.md).

## First test

The repo includes a native Windows UI test target in [targets/number_target](targets/number_target).

Build it:

```powershell
cd targets\number_target
cargo build --release
cd ..\..
```

Run it:

```powershell
.\targets\number_target\target\release\number_target.exe
```

The window shows a live integer value and includes the PID and memory address in the title.

Example agent prompt:

```text
Use the `rexd` MCP server.

Attach to the running `number_target.exe` process.
Find the displayed number in memory and change it to `100`.
Prefer using the PID or memory address shown in the window title if available.
Report the exact MCP tool calls you used and the final value you confirmed.
```

## Example workflows

Hook demo:

```powershell
npm run example:e2e
```

This launches a local sample process, attaches to it, hooks `KERNELBASE!CreateFileW`, triggers a file read, and drains captured hook events.

Example files:

- [examples/e2e-hook-createfilew.mjs](examples/e2e-hook-createfilew.mjs)
- [examples/e2e-notepad-memory-edit.mjs](examples/e2e-notepad-memory-edit.mjs)
- [examples/target-read-file.mjs](examples/target-read-file.mjs)

## Notes

- `spawn` starts a process suspended so hooks can be installed before `resume`
- `writeMemory` expects an even-length hex string without spaces
- the MCP server is the intended public integration surface
- the raw JSON bridge remains available for direct automation and debugging

## Uninstall

Remove the MCP server from Codex:

```powershell
codex mcp remove rexd
```

Remove the MCP server from Claude Code:

```powershell
claude mcp remove rexd
```

Optional local cleanup:

```powershell
Remove-Item -Recurse -Force node_modules
Remove-Item -Recurse -Force targets\number_target\target
```

## Development

Syntax check:

```powershell
npm run check
```

## Security and contribution

- [SECURITY.md](SECURITY.md)
- [CONTRIBUTING.md](CONTRIBUTING.md)
- [LICENSE](LICENSE)
