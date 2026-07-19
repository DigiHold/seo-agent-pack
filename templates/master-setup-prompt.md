# Master setup prompt

Paste everything below into Claude Code from the folder containing this pack.

---

You are setting up my autonomous SEO agent on this machine, using the SEO Agent Pack in this folder. Do it step by step and verify each piece before moving on.

1. Install the engine: copy `skills/seo-engine/` into my Claude Code skills folder so it loads as a skill. Read all four reference files so you know the method you'll be running.
2. Create my site config from `templates/site-config-template.md`: interview me for every field (domain, audience, voice, 3 to 5 topic clusters, image palette, publish policy, currency, where my blog lives and how drafts are saved there).
3. Create a .env file from `templates/env.example` and ask me to paste my four keys into it myself. Never print, copy or commit anything from that file, ever.
4. Write and test a small script that queries the DataForSEO API for keyword volume, difficulty and suggestions using DATAFORSEO_AUTH, and show me one real result.
5. Walk me through connecting the Google Search Console API for my site, step by step in plain words, and test it with one real query. It only takes a few extra minutes, and quick wins plus the Sunday optimization depend on it.
6. Write and test a script that generates one image with the Gemini image API using GEMINI_API_KEY, following the image-prompts reference.
7. Send me a test message on Telegram using TELEGRAM_BOT_TOKEN and find my chat id yourself through the bot API.
8. Write and test an auto-indexing script that reads my sitemap daily, submits new or updated published URLs to Google through the Search Console API and to Bing through IndexNow (generate the IndexNow key and place it at my site root), and once a week submits the sitemap and reports what is still not indexed.
9. Install the nightly run prompt from `templates/nightly-run-prompt.md`, filling in my paths.
10. Create the schedule from `templates/schedule-examples.md` (article runs Monday, Wednesday, Friday; page runs Tuesday, Thursday, Saturday; the optimization run Sunday), but leave it disabled and show me everything first.
11. Finish with one manual test run of an article night and show me the draft it produced. Never publish anything live.
