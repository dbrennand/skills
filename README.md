# dbrennand/skills | Codex Plugin Marketplace

This repository is a Git-backed, repo-local Codex plugin marketplace for [dbrennand/skills](https://github.com/dbrennand/skills).

It is used to store personal Codex skills as plugins.

Publishing this repository to GitHub helps with distribution and version control, but Codex still installs this marketplace from a local checkout rather than directly from the GitHub URL.

## Install in Codex

1. Clone the repository locally:

```bash
git clone https://github.com/dbrennand/skills.git
```

2. Add your local checkout as a Codex marketplace.

Use the root of your clone, not the `.agents/plugins` directory, because Codex resolves plugin sources from the marketplace root:

```bash
codex plugin marketplace add /path/to/skills
```

Example:

```bash
codex plugin marketplace add ~/src/skills
```

3. Install the plugin from this marketplace:

```bash
codex plugin add pr-description-markdown@dbrennand-skills
```
