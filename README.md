# SEO Agent

An autonomous SEO agent that drafts articles and pages for my sites every night, on a normal Claude subscription. This repo is the exact engine it runs on, cleaned of my own site names. It is not a demo or a template I wrote for GitHub. These are my working files, two months of corrections included.

## What it does

Every night a scheduled Claude Code run picks one keyword from real search data, checks it against the live top of the SERP, writes the piece under a strict anti-slop rulebook, generates its images, and saves a draft for a human to publish. On Sunday it re-reads Google Search Console and rewrites whatever underperforms. It runs on my Mac, costs the flat price of a Claude subscription with no per-token API bill for the writing, and reports to me on Telegram every morning.

Over the last two months it has drafted 70 articles and 65 pages across 4 sites. I review every piece before anything goes live.

## Why this is not an AI-spam machine

That is the first objection, so here it is up front. The whole rulebook exists to stop the agent from shipping generic content.

- Keywords come only from DataForSEO and Search Console, never invented. Zero verified search volume means no article.
- Before writing, the agent reads the current top 3 pages, maps what they cover, lists what they miss, and has to write one sentence on why its version deserves to beat them. If it cannot write that sentence, it drops the topic.
- A banned-pattern pass and a humanizer pass, the second built from Wikipedia's "Signs of AI writing" page, run on every single draft.
- Every price, date and number is verified against a live source during the run, or it gets cut.
- Nothing publishes on its own. Every piece stays a draft until a human approves it.

It will not turn a thin site into an authority overnight, and it can still write a mediocre first draft. It just will not ship the slop most autonomous setups produce.

## What is inside

- `skills/seo-engine/` holds the method: keyword selection through 5 gates, the competitor pass, quick-win hunting from Search Console, the writing rules, the 500-word image-prompt method, and the nightly run protocol.
- `templates/master-setup-prompt.md` is one prompt you paste into Claude Code so your own agent builds the whole system, interviews you for your site config, writes the helper scripts, and stops for your review.
- `templates/` also holds the site-config template, the nightly run prompt, the four-secret `env.example`, and the cron and launchd schedule.

## How to start (about an hour, no coding required)

You do not build this by hand. You paste one prompt and your own Claude Code agent builds it for you, from the files in this repo.

1. Install Claude Code from Anthropic's site and log in with a Claude subscription. The Pro plan is enough for one site.
2. Create four accounts and grab the keys: a DataForSEO account (keyword data), Google Search Console connected to your site (free), a Gemini API key (images), and a Telegram bot through BotFather (free morning reports).
3. Download this repo with `git clone https://github.com/DigiHold/seo-agent-pack`, then open Claude Code inside that folder.
4. Copy the entire contents of `templates/master-setup-prompt.md` and paste it into Claude Code. The agent reads the method files, installs the engine, interviews you about your site, writes its own helper scripts, and asks you to paste your four keys into a local `.env` file. Nothing leaves your machine.
5. When it finishes, it runs one test article and shows you the draft. Read it, fix anything you dislike in the rule files, then tell the agent to enable the nightly schedule.

From that night on it runs by itself, and you review one draft per morning on Telegram. The only slow part of setup is Search Console API access on Google's side, and the agent walks you through it step by step in about 15 minutes.

## Safety defaults

Every piece the agent writes saves as a draft, so a human always presses publish. The agent never prints, copies or commits anything from `.env`. Each run has a hard timeout and writes one status line to a dated log. Credentials are scoped to the minimum each job needs. Keep all of this in place.

## Honest limitations

- First-week drafts are usually mediocre. The system gets better because every correction you write into the engine files is permanent.
- It needs real Search Console history to find quick wins, so a brand-new site with no data gets less out of it at the start.
- It optimizes around one site's topic clusters. Point it at unrelated subjects and the quality drops.

It is MIT licensed and built in public by Nicolas Lecocq. If you build on it, I would like to hear how it goes.
