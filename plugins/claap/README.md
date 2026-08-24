# Claap — AI agents for revenue teams

Ready-to-run Claude skills that work on your real **Claap** call recordings.
Each agent pulls transcripts and deal context via the Claap MCP server and turns
meetings into action: win/loss readouts, battlecards, onboarding decks, customer
stories, daily recaps and more.

Mirrors the live [Claap Agent Gallery](https://claap.io/agent-gallery).

## Install

```
/plugin marketplace add claap-marketing/claap-skills
/plugin install claap@claap-skills
```

The **Claap** MCP server (`https://api.claap.io/mcp`) ships with the plugin — on
first use each agent asks you to authorize it (OAuth) against your Claap
workspace. Nothing else to install.

## No Claap account? See it work first

Every agent has a **built-in sample sales call**. With no Claap data connected,
just run an agent and it produces the complete, real output on that sample — so
you can see exactly what you'd get before connecting anything.

## Agents

| Skill | What it does | Connect |
|---|---|---|
| 🥊 `battlecard-generator` | Create competitor battlecards on demand — grounded in real prospect quotes from Claap recordings and verified against your product docs + the competitor's. | Claap · Notion |
| 🎓 `customer-onboarding-deck-builder` | Turn every closed-won deal into persona-specific onboarding decks — IC sales, managers, CSMs, product, execs — grounded in real CRM data and Claap transcripts. | Claap · HubSpot · Google Drive |
| ✍️ `customer-story-writer` | Generate a cinematic customer story page from every won deal — hero, quotes carousel, ROI metrics, deal timeline — and publish directly to your CMS. | Claap only |
| 📈 `objection-dashboard-builder` | Turn scattered objection data into an interactive monthly dashboard — verbatim quotes, category trends, and coaching insights, branded to your design system. | Claap · Lovable |
| 🧑‍🏫 `sales-deck-builder` | Build a fully branded, prospect-specific sales deck in minutes — pulling pain points, quotes, and pricing context from every Claap call. | Claap · Google Drive |
| 🎯 `sales-meetings-daily-recap` | Get a structured Slack recap every morning of yesterday's sales calls — objections, competitor mentions, and product signals, each deep-linked to the exact moment in the recording. | Claap · Slack |
| 🤝 `sales-cs-handover` | Automatic one-page handover for every closed deal — stakeholders, commitments, success criteria, and risks pulled from every sales recording in the cycle. | Claap · Notion (optional) · HubSpot/Salesforce (optional) |
| 🎭 `ramp-trainer` | Cut ramp time for new reps: mines your closed deals, turns them into voice roleplays where an ElevenLabs AI plays the buyers your team actually faces, scores every drill, and says when a rep is ready for a live call. | Claap · ElevenLabs (optional) |
| 📊 `win-loss-analyzer` | Run a weekly win/loss readout — pulls closed deals from your CRM, mines reasons from Claap transcripts, ranks themes, and posts the digest to Slack. | Claap · HubSpot · Slack |

"Connect" shows what each agent uses. **Claap is bundled**; anything else (CRM,
Slack, Notion, Google Drive…) is optional — you connect it yourself, and the
agent still delivers an in-chat artifact if it's missing.

## Usage

Ask Claude to run an agent by name, e.g. *"Run the weekly win/loss analysis on
the last 30 days."* Replace any `<placeholder>` values (CRM filter, workspaces,
channels) the first time. To run on a cadence, save the agent as a Scheduled
agent in Claude.
