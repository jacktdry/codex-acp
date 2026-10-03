# jacktdry fork notes

Upstream: agentclientprotocol/codex-acp

## Local lifecycle patch

Branch: fix/session-lifecycle-recycle

Purpose:

- Codex app-server currently keeps an unsubscribed thread runtime alive for a grace period.
- Loaded thread runtime can keep stdio MCP / REPL child processes alive after ACP session/close.
- This fork recycles the ACP-owned dedicated Codex app-server after the final local ACP session closes.
- Runtime-only ACP gateway/provider routing is copied to the replacement client before initialize so recycling does not silently reset routing.

The patch deliberately does not archive/delete the remote thread. The transcript remains resumable.

## Retirement condition

Remove the recycle workaround when upstream Codex exposes and codex-acp adopts a reliable per-thread unload primitive that immediately releases thread-owned MCP/runtime resources.

## Validation

Required before updating the installed fork:

- session-close targeted tests
- typecheck
- build
- real process smoke test when changing process lifecycle behavior
