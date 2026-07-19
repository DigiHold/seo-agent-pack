# Run protocol: one site, one day

The cron wrapper calls the agent headless with: the **site** (from your site list) and the
**day** (monday … sunday). Load this engine + that site's config, then run.

## 0. Pre-flight (every run, before anything)

1. **Quota guard.** The wrapper already checked remaining plan/API budget; if it told you budget is
   low, write `SKIPPED (quota)` to the status file and stop.
2. **Fetch the sitemap** at the config's `sitemapUrl` (WebFetch). **This applies to EVERY site, on
   every single run.** You must always see the existing pages/articles to avoid duplicate keywords
   and cannibalization.
   - **If it fails / is unreachable / empty → STOP this site for this run.** Write
     `SKIPPED (sitemap unreachable)` to the status file, send the notification note, and exit. Do NOT
     write or optimize anything.
   - A site that is temporarily offline simply skips every run until its sitemap resolves again; when
     it comes back, the next run proceeds automatically, no change needed. The same safeguard protects
     every other site if its sitemap is ever down.
3. From the sitemap, build the list of existing URLs/slugs for the duplicate check.
4. **DRAFT CHECK (mandatory: the sitemap only lists PUBLISHED content; drafts cannibalize too).**
   The sitemap excludes drafts/unpublished, so you must ALSO enumerate every **draft / scheduled /
   pending / not-yet-published** article for the site from its authoritative source, and add their
   titles + slugs + target keywords to the existing-content list:
   - **Code-registry blog:** read your blog registry file, **every** entry regardless of `status`
     (`draft`/`scheduled`/`published`), plus the content files already on disk.
   - **Registry + database blog:** read the full registry AND the DB (a registered post with no
     published DB row is a live draft), plus the existing content directories.
   - **WordPress:** query WP REST for **all statuses including drafts** (needs auth):
     `GET {WP_URL}/wp-json/wp/v2/posts?status=publish,future,draft,pending&per_page=100&_fields=title,slug,status`
     (and the same for `/pages` if relevant). Authenticate with an application password.
   Your day's keyword must not overlap or cannibalize **any** item in this combined list (published
   **or** draft). If the closest match is a draft on the same topic, pick a different angle/keyword or
   skip; never ship a second article that competes with an existing draft.

## 1A. Article day (Mon / Wed / Fri)

1. Run `build_onpage_brief` **LIVE** for one fresh **informational** keyword (see keyword-method.md), the same live-research model as page days. Pick the keyword now, today, from real data; do not work from a pre-locked list.
   - **Avoid duplication/cannibalization:** the day's keyword must be new against the **combined existing-content list from pre-flight steps 3 + 4: published (sitemap) AND drafts (registry / CMS all-statuses)**, so it never competes with a live page, a sibling from this week, or an existing draft. No gap map and no Sunday candidate list; the keyword is found fresh today.
2. Confirm the brief: real primary + secondary keywords with **real search volume** (volume floor + anti-junk rule), intent = informational, new (no existing article OR draft on it per the combined published+draft list from pre-flight steps 3-4). **Then do keyword-method Step 5 (mandatory): WebFetch the top-3 ranking pages, map their coverage, list their gaps/weaknesses, and write down the differentiation angle + 2-3 concrete value-adds. Do NOT start writing until you can state why this article will out-value the current top 3.** Cover the baseline, fill the gaps, lead with the angle.
3. Write the article via the config's **voice guide** + `references/writing-rules.md` (blocking checklist). On a bilingual site, write the primary language first, then the second.
4. Generate all images (featured + one per H2) with the **site's own image pipeline**: detailed prompts, keyword-slug filenames, AVIF + WebP-OG, keyword alts. **The filename of each generated image MUST exactly match an image reference in the body** (featured + one per H2). Per pipeline type:
   - **Local-files site:** your image-generation script writes files into the site's public assets dir (committed + served with the site).
   - **CDN/bucket site:** use the script that actually **uploads** to the bucket. A generator that only writes local files means every image 404s in production; never use it for a CDN-served site.
   - **CMS site:** your publish script uploads the AVIF in-page images + the WebP OG to the media library.
5. Internal linking pass (relevance-gated, cooldown-aware, same-locale on bilingual sites).
6. **SELF-REVIEW GATE (mandatory):** run the self-review checklist at the end of `writing-rules.md` on the finished draft. Fix anything that fails and re-check. **Then the QUALITY GATE (one bounded pass):** review the finished draft for AI-citability (would an AI engine quote the opening sentence? is each section extractable as a clean capsule?) and for E-E-A-T (experience, expertise, authoritativeness, trust signals: author expertise, a date, an honest recommendation), and apply the HIGH-impact fixes only: a first sentence a model won't quote, a missing E-E-A-T signal, a passage that should be a clean quotable capsule. Cap: ONE corrective pass, never loop or rewrite wholesale. Do NOT proceed until every item passes.
7. Save as **draft** to the config's target (registry entry with status `draft` + the content file; a DB row with status draft; or the CMS via REST with status draft).
8. **IMAGE-VERIFICATION GATE (mandatory, never skip):** confirm **every** image referenced in the finished article actually resolves, before writing the status line. Local-files site → each file exists on disk (and is in the commit); CDN site → `curl` each CDN URL (featured `.avif` + `.webp` + every section `.avif`) returns **200**; CMS site → each in-content image + the featured return **200** from the media URL. If **any** image is missing / 404, regenerate + re-upload (re-run the site's image script) until all pass. **Never report the run done with a broken or missing image.** Then build/verify (the config says how) and write the draft URL + "self-review passed; images verified" to the status file.

## 1B. Page day (Tue / Thu / Sat)

1. `build_onpage_brief` live → a strong **commercial/transactional** page keyword for the site.
2. **Sitemap check**: does an existing page target this keyword/topic?
   - **Exists + performing** (good clicks/impressions in GSC) → no action; try the next candidate keyword. Only conclude "no action needed" if every reasonable candidate is already well covered.
   - **Exists + underperforming** → optimize it. Run **Step 5** on the page, then do real **content work**, not just title/meta: **add a FAQ (accordion/section) if it has none**, add the sub-topics/questions the top-3 cover that the page misses, deepen thin sections with specifics/examples/data, fix any intent mismatch (re-angle), refresh internal links, and update title/meta. **Applied LIVE in place on every site** (commit + push for code-based sites; a CMS edit keeping `status:publish`, never unpublish/draft a ranking page). Respect the **cooldown** (skip if changed in the last ~1 month; note "waiting for data"). Keep it genuinely more useful, no filler.
   - **No existing page** → create a new page (your bespoke per-site design; pages publish per your config's publish policy).
3. Whether optimizing or creating, do keyword-method **Step 5** first: read the top-3 ranking pages, map coverage + gaps, and write the differentiation angle. An optimized page must end up clearly more useful than the current top results, not just tweaked.
4. Same writing checklist + images + internal linking, then the **SELF-REVIEW GATE** (writing-rules.md) + the **QUALITY GATE** (one bounded pass of the AI-citability + E-E-A-T review, apply HIGH-impact fixes only) before save. Save + status URL ("self-review passed; quality gate passed").

## 1C. Optimization run (job `sunday` = full; job `optimize` = pages only)

GSC-driven page optimization runs **twice a week**, both improving what already exists (no new article, no keyword candidates, no gap map):
- **`sunday`** (Sunday, the site's usual morning slot) → the **FULL** routine: page optimization (step 1) **plus** the internal-linking pass (step 2) **plus** the summary (step 3).
- **`optimize`** (midweek, e.g. Wednesday at 3am) → **step 1 ONLY**: page optimization + the per-site report. It does **NOT** run the internal-linking pass (step 2) or any other Sunday task; it only optimizes the pages.

Each run optimizes **up to 5 pages**; the per-page **~1-month cooldown** means the two days work on **different** pages (the midweek run picks up the candidates Sunday didn't reach), covering up to ~10 distinct pages a week.
For the site:
1. **GSC audit + optimize existing content** (your GSC query script). The key bucket is **OUTSIDE TOP 10 → OPTIMIZE**: indexed content pages with real impressions whose best position is **> 10** (your script should exclude utility/auth/legal/taxonomy URLs). **Optimize up to 5 of these pages/articles this run by default, OR up to 8 when there is a big backlog** (adaptive per site): read the candidate count the OUTSIDE TOP 10 bucket prints; if it shows **≥30 eligible candidates**, do up to **8** this run to chew through the backlog faster; otherwise cap at **5**. Biggest opportunity first: actually do the work, do not just list it. The ceiling (5 or 8) is a MAXIMUM, never a quota: **quality and depth always beat hitting the number**. Do as many as there are genuine candidates AND as many as can each be done PROPERLY, never rush a shallow rewrite to fit more in (4 done well beats 8 done badly). (Optimizing existing indexed pages at this pace is safe with Google; the only hard limits are the per-page ~1-month cooldown and leaving winners alone, both below.) Rules for picking and doing each:
   - **Only real, indexed content.** A page is eligible ONLY if it has **impressions** (indexed + real demand). **NEVER optimize a 0-impression page**: zero impressions means a new page / new site with no data to work from, so leave it untouched. **NEVER optimize a utility page** (contact, sign-in/up, account, cart, checkout, legal, privacy, terms, cookies, search, tag/category/author, or bare listing/index/forum roots like `/blog/`, `/community/`): only real articles and real landing / feature / comparison pages. **Blog / news ARTICLES are PRIME targets, optimized exactly like pages** (a live article stuck at position 15-40 for a high-impression query has huge upside, often more than a commercial page). Pages AND articles both get optimized; only the bare `/blog/` index listing is skipped, never an individual article.
   - **Leave winners alone** (position ≤ 4 with clicks): do NOT re-optimize a page that already ranks well, the downside outweighs the upside. And respect the **~1-month cooldown**: never re-touch a page changed in the last ~1 month, note "waiting for data" and move on.
   - **Each optimization is competitor-driven.** Take the page's top query, pull the **live page-1 competitors** (your DataForSEO SERP call) and **scrape their content** (`on_page/instant_pages`, or WebFetch the real top-3), map what they cover that your page is missing, then rewrite your page to **beat them**: find a sharper / different angle from the current top 3, add the sub-topics or questions they answer that you don't, add a comparison table or FAQ where it genuinely helps, fix any intent mismatch (re-angle), refresh internal links, tighten title + meta. A snippet-only fix (rewrite title + meta) is enough ONLY when the page already ranks but earns no clicks.
   - **WORD COUNT: target 2200-2550 words, HARD MAX 3000.** The sweet spot is **2200-2550 words**; never exceed 3000. Do NOT bloat a page from repeated weekly optimization: if a page is already at/over that band, you must **re-angle by substitution**: rewrite or REPLACE weak / dated / redundant passages and **trim useless text to make room** for stronger, fresher content, never just pile more on top. A 4000-word page produced by optimizing the same page three times is a failure; keep it tight in the 2200-2550 band.
   - Apply every change **LIVE in place, to PRODUCTION**: optimizing on staging is pointless (it never affects rankings) and is FORBIDDEN. Commit + push to the **live/production branch** (never a staging branch), or for a CMS edit the page keeping `status:publish` (never unpublish a ranking page). The change must be live on the real domain or it did nothing.
   - **Build the report (mandatory).** As you optimize, record per page exactly what you changed, and write a clear per-site report to **`/tmp/seo-report-<site>.txt`** in this EXACT shape (it is sent as your notification after the run). Use the **FULL https URL** (never a relative path), the target keyword in quotes, the GSC line, and the changes as indented `-` bullets:
     ```
     <Site> - <N> page(s) optimized:

     1. https://example.com/free-tools/best-time-to-post
     • Keyword: "best time to post"
     • GSC: pos 14.8, 292 imp, 2 clicks
     • Changes:
           - Added 3 industries to match the top competitor's 8-industry coverage (we had 5)
           - Added 2 internal links (Algorithm 2026, Scheduling Tools)
           - Updated dateModified

     2. https://example.com/<next page>
     • Keyword: "<target keyword>"
     • GSC: pos <x>, <y> imp, <z> clicks
     • Changes:
           - <change 1>
           - <change 2>
     ```
     Always the full https URL on the numbered line. If nothing was eligible this week (every candidate is 0-impression / utility / in cooldown / already winning), write that one honest line to the file instead.
2. **Internal linking pass** (job `sunday` ONLY: SKIP this on the midweek `optimize` run) across the whole sitemap: find relevance-gated opportunities to link related pages/articles together (strengthen topical clusters), same-locale on bilingual sites, descriptive anchors. Apply the best ones live (respecting the cooldown).
3. Write a summary to the status file (what was optimized, what was left alone, what links were added).

(Sunday writes NO keyword queue and NO gap map: every weekday's article and page does its own live keyword research that day.)

## 2. Status file + notify

- Each run appends one line to your status directory, e.g. `status/<YYYY-MM-DD>.txt`:
  `<site> | <job> | <result>`. The notification body IS these lines verbatim, so the result must be
  **scannable and start with a clear ACTION LABEL**, then the FULL clickable https URL(s), then a short
  what/why. Use these labels:
  - Article day → `NEW ARTICLE (draft) - <full url(s)> - <topic, keyword> - <publish action>`
  - Page day, created → `NEW PAGE (live|draft) - <full url> - <topic, keyword>`
  - Page day, optimized → `OPTIMIZED PAGE - <full url> - <exactly what was improved + the GSC reason>`
  - Page day, nothing to do → `PAGE - no change needed (all strong candidates already rank well)`
  - Sunday → `AUDIT - <fixes applied + urls> · LINKS - <internal links added>`
  - Skipped/blocked → `SKIPPED (<reason>)`
  **Always real `https://` URLs, never bare paths** (the body's links are tappable; the notification's
  Click opens the first one). So the morning notification tells YOU, per site, exactly what was
  created or improved and the link to review it.
- A dedicated **notify job** (cron, shortly after the runs, e.g. ~07:45) reads the day's status file and sends ONE
  Telegram message with all results. You can reply in that Telegram chat ("publish …", "change …") and a
  listener daemon runs the agent headless on your machine to carry it out. Each draft line states the right publish action per YOUR publish policy for that site:
  - **Draft the agent may publish on your reply** → "reply to publish" (the agent flips draft→published when you answer).
  - **Draft you publish yourself** → "review + publish in your site admin / CMS admin".
  - Live items (pages your policy allows the agent to ship live) just show the live URL.

## Guardrails recap
- Drafts-only where the config says draft; never auto-publish those.
- One site at a time (the wrapper staggers the runs, e.g. hourly morning slots).
- Sitemap unreachable or keyword-data/GSC empty → skip + report, never guess.
- Quality over volume; beat the current SERP or skip.
