# Claap — AI agents for revenue teams

Nine ready-to-run AI agents that turn your **Claap call recordings** into the things revenue teams actually need: win/loss readouts, competitor battlecards, prospect-specific sales decks, onboarding decks, customer stories, objection dashboards, daily call recaps, sales→CS handovers, and pre-call dry runs against a voice AI that plays your actual prospect.

Each agent reads transcripts and deal context through the [Claap MCP server](https://help.claap.io/en/articles/11786341-connect-claude-ai-with-claap-s-mcp-server) (bundled, one-click OAuth) and delivers a finished, branded artifact. Mirrors the live [Claap Agent Gallery](https://claap.io/agent-gallery).

## Install (Claude Code)

```
/plugin marketplace add claap-marketing/claap-skills
/plugin install claap@claap-skills
```

That's it — the Claap MCP ships with the plugin. The first time you run an agent, Claude asks you to authorize Claap (OAuth) against your workspace. Nothing else to set up.

## No Claap account? See it work in 30 seconds

Every agent has a **built-in sample sales call.** With nothing connected, just run one — it produces the complete, real output on that sample, so you can see exactly what you'd get before connecting a thing. When you're ready, connect Claap and run it on your own calls.

<!-- TODO: drop a short GIF of an agent run here — e.g. "ask → branded win/loss deck". Save it to docs/demo.gif and uncomment: -->
<!-- ![Claap agent in action](docs/demo.gif) -->

## The agents

Run an agent just by asking for it in plain language. **Claap is bundled**; anything in "Connect" beyond Claap is optional — connect it yourself and the agent still delivers an in-chat artifact if it's missing.

| Agent | What you get | Try saying | Connect |
|---|---|---|---|
| 📊 `win-loss-analyzer` | Weekly win/loss readout, themes ranked, posted to Slack | *"Run the weekly win/loss analysis on the last 30 days."* | Claap · HubSpot · Slack |
| 🥊 `battlecard-generator` | Competitor battlecard grounded in real prospect quotes | *"Make a battlecard for Gong."* | Claap · Notion |
| 🧑‍🏫 `sales-deck-builder` | Branded, prospect-specific sales deck | *"Build a sales deck for Acme."* | Claap · Google Drive |
| 🎓 `customer-onboarding-deck-builder` | Persona-specific onboarding decks from a won deal | *"Create onboarding decks for the Acme account."* | Claap · HubSpot · Google Drive |
| ✍️ `customer-story-writer` | Cinematic customer story page from a won deal | *"Write a customer story for the Acme deal."* | Claap only |
| 📈 `objection-dashboard-builder` | Interactive monthly objection dashboard | *"Build this month's objection dashboard."* | Claap · Lovable |
| 🎯 `sales-meetings-daily-recap` | Morning Slack recap of yesterday's calls, deep-linked | *"Recap yesterday's sales calls."* | Claap · Slack |
| 🤝 `sales-cs-handover` | One-page sales→CS handover for a closed deal | *"Create the sales-to-CS handover for the Acme deal."* | Claap · Notion · CRM |
| 🎭 `pre-call-trainer` | Practice call against a voice AI playing your actual prospect, then coaching | *"Prep me for my 3pm with Acme — dry run first."* | Claap · ElevenLabs |

Want it on a cadence? Save any agent as a **Scheduled agent** in Claude and it runs itself (e.g. the daily recap every morning).

## Install on other agents

These are standard [Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — they work anywhere the standard is supported.

### Cursor

Install from the Cursor Marketplace, or add manually via **Settings → Rules → Add Rule → Remote Rule (GitHub)** with `claap-marketing/claap-skills`.

### npx skills

```
npx skills add https://github.com/claap-marketing/claap-skills
```

### Clone / copy

Clone this repo and copy the agent folders into your agent's skills directory:

| Agent | Skills directory | Docs |
|---|---|---|
| Claude Code | `~/.claude/skills/` | [docs](https://code.claude.com/docs/en/skills) |
| Cursor | `~/.cursor/skills/` | [docs](https://cursor.com/docs/skills) |
| OpenAI Codex | `~/.codex/skills/` | [docs](https://developers.openai.com/codex/skills/) |
| OpenCode | `~/.config/opencode/skills/` | [docs](https://opencode.ai/docs/skills/) |

The agent folders live in [`plugins/claap/skills/`](plugins/claap/skills/).

## What you'll need

- A **Claap** account with call recordings (bundled MCP, authorized on first use) — or just use the built-in sample call to try it.
- For some agents, the extra MCP servers listed in "Connect" above (your CRM, Slack, Notion, Google Drive). Each agent states what it uses and degrades gracefully when something isn't connected.

If you have Claap but the MCP isn't connected yet, follow the [connector setup](https://help.claap.io/en/articles/11786373-using-claap-s-mcp-server).

## Resources

- [Claap Agent Gallery](https://claap.io/agent-gallery) — the same agents, on the web
- [Connect Claude with Claap (MCP)](https://help.claap.io/en/articles/11786341-connect-claude-ai-with-claap-s-mcp-server)
- [Agent Skills standard](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

---

## For the Claap team — standalone design skills

These live in [`design-skills/`](design-skills/) and are **internal tooling**, not part of the agents plugin above. Each ships as an unpacked folder and a `.skill` zip you upload to Claude.ai as a personal skill.

- **`claap-design-system`** — the Claap design system as a Tailwind-first reference (tokens, typography, component recipes, motion) so Claude artifacts feel native to Claap.
- **`design-system-extractor`** — extracts a full design system from any live product (website or Figma) into a drop-in design-system skill.
- **`extract-branding-theme`** — reverse-engineers any `.pptx` into a structured JSON design system and saves every logo/image to disk.

## License

[MIT](LICENSE)
