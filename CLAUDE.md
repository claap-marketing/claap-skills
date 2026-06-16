# Claap Skills — project guide for agents

## What this is

A marketplace-ready collection of **agent skills for revenue teams**, built on Claap call recordings. It ships one plugin (`claap`) plus a few standalone internal design skills. This is a **content/config project** — no application code, just `SKILL.md` files and plugin manifests that guide agent behaviour.

## Source of truth

The eight prospect-facing agents are **generated from the live [Claap Agent Gallery](https://claap.io/agent-gallery)** (a Webflow CMS). A build script reads each agent's prompt from its gallery article and emits the plugin. Edit the gallery, then rebuild — do not hand-edit generated files. See `CONTRIBUTING.md` for the generated-vs-hand-maintained split.

## Repository structure

```
plugins/claap/
  .claude-plugin/plugin.json   # Claude Code plugin manifest (generated)
  .codex-plugin/plugin.json    # Codex plugin manifest (generated)
  .mcp.json                    # bundled Claap MCP (https://api.claap.io/mcp)
  README.md                    # plugin overview + agent table (generated)
  skills/<agent>/SKILL.md       # one folder per agent (generated)
.claude-plugin/marketplace.json # Claude Code marketplace entry (generated)
.cursor-plugin/                 # Cursor marketplace + plugin (generated)
.agents/plugins/marketplace.json # Codex marketplace entry (generated)
design-skills/                  # standalone internal skills (hand-maintained)
README.md, LICENSE, CONTRIBUTING.md, .github/  # hand-maintained
```

## Design conventions for agents

- **Never block the output.** Claap is the only required tool; every other MCP (CRM, Slack, Notion, Drive…) is optional. If it's missing, deliver an in-chat artifact and offer to connect it later.
- **Demo mode.** With no Claap data, run on the agent's bundled sample call and deliver the complete output — never tell the user to go set up Claap first.
- **Default-first inputs.** Pick sensible defaults, state them in one line, proceed. Only hard-stop on a genuinely required input you can't infer.
- **Evidence only.** Quote real transcript moments; never invent reasons, quotes, or numbers.
- **Artifact-first.** The deliverable is always a Claude artifact (React/HTML/doc/file). Writing to an external tool is optional and needs a quick confirm.

## CI

`.github/workflows/ci.yml` checks required root files exist and every skill under `plugins/claap/skills/` has a `SKILL.md` with `name:` and `description:` frontmatter. No build/test step — validation is structural.
