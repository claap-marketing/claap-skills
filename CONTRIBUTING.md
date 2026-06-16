# Contributing

The agents in this repo are **generated from the live [Claap Agent Gallery](https://claap.io/agent-gallery)**, not hand-written. This keeps the plugin and the public gallery as one source of truth.

## How the plugin is built

Each agent's prompt lives in its Agent Gallery article (in a fenced code block). A build script reads the gallery, wraps each prompt in a `SKILL.md`, and emits the full plugin + marketplace configs. To change an agent's behaviour, **edit the gallery article**, then re-run the build — don't hand-edit the generated `SKILL.md`, `plugin.json`, `marketplace.json`, or the plugin `README.md`.

Generated (do not hand-edit):

- `plugins/claap/skills/*/SKILL.md`
- `plugins/claap/README.md`
- `plugins/claap/.claude-plugin/plugin.json`
- `plugins/claap/.codex-plugin/plugin.json`
- `.claude-plugin/marketplace.json`, `.cursor-plugin/*`, `.agents/plugins/marketplace.json`

Hand-maintained:

- `README.md` (root), `LICENSE`, `CONTRIBUTING.md`, `CLAUDE.md`, `.github/`
- `design-skills/` (the standalone internal skills)

## Skill anatomy

Each agent is a single `SKILL.md` in its own folder:

```
plugins/claap/skills/<agent-name>/SKILL.md
```

Required YAML frontmatter:

```yaml
---
name: <agent-name>          # lowercase, hyphens only
description: ...            # what it does + when to use it (trigger keywords)
---
```

Every agent should:

- **Lead with the "read first" block**: safe-to-run, default-first inputs, never block on a missing optional tool, always deliver a Claude artifact.
- **Run in demo mode** on a bundled sample call when no Claap data is connected.
- **Quote real evidence** from transcripts — never invent reasons, quotes, or metrics.

## Standalone design skills

`design-skills/` holds internal skills used by the Claap team (design-system tooling). Each ships as both an unpacked folder and a `.skill` zip. They are **not** part of the prospect-facing plugin.

## Pull requests

1. Make the change in the gallery (for agents) or the relevant file (for everything else).
2. Re-run the build if you touched an agent.
3. Open a PR describing what changed and why. CI checks that every skill has valid frontmatter.
