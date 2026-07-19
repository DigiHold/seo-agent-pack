# SEO Agent Pack

The actual rule files behind my autonomous SEO agent: the one that drafts articles and pages for my 4 websites every night on a normal Claude subscription, picks every keyword from real search data, and optimizes what underperforms every Sunday. Nothing here is a template I invented for this repo. These are my working files, cleaned of my own site names so you can plug in yours.

## What's inside

- `skills/seo-engine/SKILL.md` and `skills/seo-engine/references/`: the brain. The full keyword method (5 gates, competitor pass, quick wins), the writing rules (anti-slop, structure, titles, internal links), the image-prompt method (500+ word prompts, literal subjects), and the nightly run protocol (weekday routing, draft-only, Sunday optimization).
- `templates/master-setup-prompt.md`: paste this ONE prompt into Claude Code and your agent installs everything, interviews you for your site config, writes its own helper scripts, and finishes with a test run.
- `templates/site-config-template.md`: your site's identity file (domain, audience, voice, clusters, palette, publish policy).
- `templates/nightly-run-prompt.md`: the prompt your scheduler sends to the agent every night.
- `templates/env.example`: the four secrets you need (keyword data, images, Telegram). Keys go in YOUR .env only, never anywhere else.
- `templates/schedule-examples.md`: the weekly schedule as cron lines, with launchd notes for Mac.

## Quick start (about an hour)

1. Install Claude Code and log in with your Claude subscription (Pro is enough for one site).
2. Create accounts: DataForSEO, Google Search Console, a Gemini API key, a Telegram bot.
3. Open Claude Code in a folder containing this pack and paste `templates/master-setup-prompt.md`. Answer its questions and paste your keys only into the .env file it creates.
4. Review the test draft it produces, then tell it to enable the schedule.

Your first week of drafts will be mediocre, and that's normal. Every correction you write into the engine files is permanent, so the agent gets sharper every week. Mine contains two months of my corrections already, and they're all in here.

## Safety defaults (do not remove them)

Everything saves as a draft, a human presses publish. The agent never prints, copies or commits secrets. Every run has a timeout and writes a status line. Keep it that way.

Built in public by Nicolas Lecocq. Free to use and adapt (MIT). If it helps you, tell me on X how your first night went.
