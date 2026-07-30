---
name: obsidian-headless-sync
description: Work with Obsidian Headless Sync (ob CLI) — sync the LLM Wiki vault at /home/hermes/llm-wiki to the remote after any changes.
---

# Obsidian Headless Sync

Work with Daniel's Obsidian Sync vault at `/home/hermes/llm-wiki` using the headless `ob` CLI.

## Prerequisites

- `ob` CLI installed (from `obsidian-headless` npm package)
- Already logged in (`ob login`)
- Vault already set up (`ob sync-setup` already run)

## Sync Vault (one-shot)

After making any changes to the vault (creating/editing notes, renaming, deleting), **always** run sync:

```bash
cd /home/hermes/llm-wiki && ob sync
```

## Sync Vault (continuous)

Watches the vault directory for changes and syncs automatically:

```bash
cd /home/hermes/llm-wiki && ob sync --continuous
```

## Check Sync Status

```bash
cd /home/hermes/llm-wiki && ob sync-status
```

Or with JSON output:

```bash
cd /home/hermes/llm-wiki && ob sync-status --json
```

## List Remote Vaults

```bash
ob sync-list-remote
```

## List Local Vaults

```bash
ob sync-list-local
```

## Useful Flags

| Flag | Purpose |
|------|---------|
| `--path <path>` | Specify vault path (default: current dir) |
| `--json` | Machine-readable JSON output (most commands) |
| `--continuous` | Watch for file changes and sync automatically |

## Pitfalls

- Always `cd /home/hermes/llm-wiki` or use `--path /home/hermes/llm-wiki` — `ob` without `--path` uses the *current working directory*.
- `ob sync` is a one-shot command. Use `--continuous` for a long-running daemon.
- `ob sync-setup --json` disables interactive password prompts for E2E vaults, but then `--password` is required.
