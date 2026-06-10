# dbrennand/skills | Codex Plugin Marketplace

This repository is a Git-backed, repo-local Codex plugin marketplace for [dbrennand/skills](https://github.com/dbrennand/skills).

It is used to store personal Codex skills as plugins.

The current `git` plugin bundles:

- `pr-body-markdown` for polished, reviewer-friendly pull request bodies
- `conventional-commit-message` for commit messages that follow the Conventional Commits 1.0.0 specification

## Install using the Codex App

1. Clone the repository locally:

    ```bash
    git clone https://github.com/dbrennand/skills.git
    ```

2. Ask Codex to install the marketplace and plugin(s) from your local checkout.

## Install using the Codex CLI

If you prefer to run the setup yourself:

1. Add your local checkout as a Codex marketplace:

    ```bash
    codex plugin marketplace add /path/to/skills
    ```

2. Install the plugin from this marketplace:

    ```bash
    codex plugin add git@dbrennand-skills
    ```

## Run hooks locally

Use `prek` to run the Markdown hooks locally:

```bash
uvx prek run --all-files
```

If you want `prek` installed as a Git hook in the clone:

```bash
uvx prek install --prepare-hooks
```
