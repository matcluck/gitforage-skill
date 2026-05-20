# Git Forage

Git Forage is a prior-art and duplicate-work research skill for Codex and Claude Code. Use it before building new tools, plugins, libraries, integrations, or major features so agents can check registries, issue queues, pull requests, and GitHub projects before recommending whether to use, contribute, wait, build new, or take a hybrid path.

## Install

Add the marketplace, then install the `gitforage` plugin from that marketplace.

Codex:

```bash
codex plugin marketplace add matcluck/gitforage-skill
```

For a local checkout:

```bash
codex plugin marketplace add /path/to/gitforage-skill
```

Then open `/plugins`, select **Git Forage**, and install `gitforage`.

Claude Code:

```bash
claude plugin marketplace add matcluck/gitforage-skill
claude plugin install gitforage@gitforage-marketplace
```

For a local checkout:

```bash
claude plugin marketplace add /path/to/gitforage-skill
claude plugin install gitforage@gitforage-marketplace
```

## Install Shapes

The plugin layout is the source of truth:

- `.agents/plugins/marketplace.json` for Codex marketplace installation
- `.claude-plugin/marketplace.json` for Claude Code marketplace installation
- `.codex-plugin/plugin.json` and `.claude-plugin/plugin.json` for plugin metadata
- `skills/` for reusable workflow skills

- `git-forage`

## Safety Defaults

- Check registries, issues, and pull requests before searching for inspiration.
- Prefer maintained projects with permissive licenses.
- Reject copyleft, source-available, unclear, or missing licenses unless the user explicitly approves research-only review.
- Clone references into a local `inspirations/` or scratch directory only after selecting candidates worth deeper analysis.
- Extract patterns and tradeoffs; do not copy code unless license and project policy allow it.

## Structure

```text
gitforage-skill/
+-- .agents/plugins/marketplace.json
+-- .codex-plugin/plugin.json
+-- .claude-plugin/
|   +-- marketplace.json
|   +-- plugin.json
+-- skills/
    +-- git-forage/
```

## Review

After syncing local changes, review with:

```bash
git status --short
git diff
```
