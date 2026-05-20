---
name: git-forage
description: OSS prior-art and duplicate-work research, also called gitforage or Git Forage. Use before building new tools, plugins, libraries, integrations, services, or major features; before opening an upstream PR; when the user asks how other projects solve a similar problem; or when you need to find, vet, clone, and analyze GitHub projects for reusable patterns.
---

# Git Forage

Find, vet, clone, and analyze open-source projects to extract patterns you can reuse. Before that, check whether someone has already built or is currently building the same thing.

## When To Use

- Before starting a new tool, plugin, library, service, integration, or packaged ecosystem contribution.
- Before opening an upstream PR, to confirm you are not duplicating an existing PR or active fork.
- When a pain point or bug reveals a design flaw worth rethinking.
- When building or refactoring a feature and prior architecture may save time.
- When the user asks how other projects handle a similar problem.

## Workflow

### 0. Check For Existing Or In-Flight Work First

If the work targets a packaged ecosystem, search its canonical registry and its issue or PR queue before any inspiration search. The thing you are about to build may already exist as a package, open PR, active fork, closed attempt, or feature request.

Pick the right registry by ecosystem:

| Ecosystem | Search targets | Example command |
|---|---|---|
| OPNsense plugins | `opnsense/plugins` issues and PRs | `gh search prs --repo opnsense/plugins "<term>" --state all` |
| FreeBSD ports | `freebsd/freebsd-ports`, FreshPorts | `gh search code --repo freebsd/freebsd-ports "<term>"` |
| npm | npmjs.com, package repos | `npm search <term>` |
| PyPI | pypi.org, package repos | `pip index versions <name>` |
| crates.io | crates.io, package repos | `cargo search <term>` |
| Terraform | registry.terraform.io, provider repos | web or GitHub search |
| GitHub Actions | GitHub Marketplace, action repos | `gh search repos "topic:github-action <term>"` |
| Helm charts | Artifact Hub | `helm search hub <term>` |

Universal GitHub queries:

```bash
# Open and closed PRs in the registry or upstream repo.
gh search prs --repo <owner/repo> "<topic>" --state all --limit 20

# Issues describing the same feature, bug, or gap.
gh search issues --repo <owner/repo> "<topic>" --state all --limit 20

# Code search for names, conventions, or modules.
gh search code "<topic> in:path" --limit 20

# Cross-org search for standalone repos with the same purpose.
gh search repos "<topic> <ecosystem>" --sort updated --limit 20
```

For each relevant hit, capture state, age, last updated date, author, rough size, review status, license, and URL.

Decide before continuing:

| Verdict | Meaning | Action |
|---|---|---|
| `USE` | Mature, maintained, fits requirements, permissive license | Install or depend on it. Stop building a duplicate. |
| `CONTRIBUTE` | Open PR, active fork, or active issue is close to the need | Review existing work and contribute or open a complementary PR. |
| `WAIT` | Active work is likely to land soon and need is not urgent | Track the issue or PR. |
| `BUILD-NEW` | Nothing exists, license is incompatible, project is abandoned, or scope mismatches | Continue to prior-art inspiration search. |
| `HYBRID` | Existing work covers part of the scope | Build only the missing piece and integrate. |

Document the verdict and evidence. If the verdict is `BUILD-NEW`, state why the closest alternatives do not fit.

### 1. Frame The Problem Generically

Strip project-specific names and describe the engineering problem in general terms. Include:

- domain and ecosystem
- technical constraints
- must-have behavior
- non-goals
- likely search phrases

### 2. Search Broadly For Prior Art

Run 3 to 5 varied searches before selecting candidates:

- direct problem description, such as `"event-driven workflow engine"`
- technology plus pattern, such as `"typescript fan-out fan-in queue"`
- known project alternatives, such as `"alternatives to temporal"`
- ecosystem-specific searches, such as `"topic:github-action dependency update"`
- curated lists, such as `"awesome <domain>"`

If the host agent policy allows subagents and the user requested delegation, split independent searches across agents. Otherwise run the searches directly.

### 3. Vet Candidates

License is a hard gate unless the user approves research-only review:

- Accept: MIT, Apache-2.0, BSD, ISC, MPL-2.0.
- Reject for reuse: AGPL, GPL, LGPL unless compatible with the target, SSPL, BSL, Elastic License v2, unclear license, or no license.

Quality signals:

- recent meaningful activity, preferably within 6 months
- more than toy-scale usage, such as stars, forks, dependents, package downloads, or known users
- tests or examples that cover edge cases
- clean structure and readable implementation
- issues and PRs show maintainers respond to important defects

### 4. Clone And Analyze

Clone only candidates worth deeper analysis. Prefer a local `inspirations/` directory in the relevant repo when the reference should be kept, or a scratch/temp directory when it is a one-off investigation.

If adding a persistent reference:

```bash
git submodule add <url> inspirations/<name>
```

Then update or create an `inspirations/INDEX.md` in the same repo with:

- name
- license
- language/framework
- GitHub URL
- one sentence explaining why it is kept and what pattern to study

For each cloned candidate, map:

- directory structure
- architecture and key components
- files that implement the relevant pattern
- tests or examples that show expected behavior
- tradeoffs, edge cases, and design choices

### 5. Recommend A Path

Report findings in this shape:

| Field | Detail |
|---|---|
| Verdict | `USE`, `CONTRIBUTE`, `WAIT`, `BUILD-NEW`, or `HYBRID` |
| Evidence | Registry, issue, PR, and repo links checked |
| Source repo | GitHub URL |
| License | Verified license and reuse status |
| Relevant files | Paths within the candidate repo |
| How it works | Architecture and key design decisions |
| How to adapt | What to borrow and what to change |
| Tradeoffs | What improves and what becomes harder |
| Next step | Install, contribute, wait, build, or prototype |

## Operating Rules

- Phase 0 is mandatory for ecosystem-targeted work.
- Search both issues and PRs. Closed PRs often reveal failed attempts and maintainer constraints.
- Prefer maintained projects with permissive licenses and useful tests.
- If two projects solve the same problem differently, compare both instead of picking the first plausible result.
- Do not copy code by default. Extract patterns, data models, workflows, API shapes, and test ideas.
- Keep private or customer-specific search terms out of public notes.
- If a candidate is rejected, preserve the reason so future agents do not repeat the same search.
