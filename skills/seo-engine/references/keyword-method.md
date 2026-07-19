# build_onpage_brief: the keyword method

Run this with the site's params (domain, GSC siteUrl, location, language from your config).
Use your keyword-data client (e.g. a DataForSEO script) and your GSC query script.

## Step 0a: Relevance gate (HARD filter, before anything else)
A keyword is eligible ONLY if it clearly serves THIS site's offer and would bring a real
potential customer. Reject anything off-topic or off-funnel, even if volume/CPC look attractive.
- Candidate keywords come **only from your site's allowed topic lanes** (define these in your site config).
- It must pass: "would someone searching this plausibly become a client of this business?" If no → drop it.
- It must NOT be in your forbidden lanes (e.g. topics that belong to a different product of yours, or adjacent industries you don't serve).
- Watch context, not just the word: for a web-design agency site, `website for artisans` is RELEVANT (an artisan
  is a client), but bare `artisan`, `artisan training`, `artisan recipes`, etc. are OFF-TOPIC: reject them.
- When unsure whether a keyword fits the business, drop it and pick another. Never write off-topic content.

## Step 0b: Data sanity, volume floor + anti-junk (never ship garbage)
Real searched keywords only. Before accepting any candidate:
- **Volume floor:** the candidate must have a **real, verified `search_volume` from `keyword_overview`** (not 0/None). Treat very thin terms with caution; if a whole probe only yields ~0-10 volume, re-seed with a better candidate rather than shipping a dead keyword. (Low-volume but easy + high-funnel terms are OK *if* relevant: judge value, not just volume.)
- **Anti-junk / re-seed:** `keyword_suggestions` and a competitor's `ranked_keywords` can return noise: either off-topic global high-volume terms (e.g. "chatgpt", "facebook", "free logo", "translate") or the fallback list when a seed is too thin. If the returned set is clearly unrelated to the seed/business, **discard it and re-seed** with a cleaner candidate from your topic lanes. Never pass that noise through as a keyword.
- **Trust the right signal:** when the #1 organic result is a huge multi-topic domain (gov/marketing megasite), its `ranked_keywords` are polluted; rely on the SERP titles + `keyword_suggestions` + `keyword_overview` instead.
- Every shipped keyword must survive Step 0a (relevance) AND have verified volume + verified intent. If in doubt, drop it.

## Step 0: Intent classification (routing gate)
Classify the candidate keyword and route it:
- **Informational** ("how to…", "how much…", "X or Y", "why…", "what is…") → **blog article** (article days).
- **Commercial / transactional** ("price…", "pricing", "agency…", "buy", "alternative", "vs", "best … tool") → **page** (page days).
On an article day only informational keywords are eligible; on a page day only commercial ones.
Confirm intent against the live SERP: a SERP full of guides = informational; full of service/agency/product pages = commercial.

## Step 1: SERP reverse-engineering (strongest signal)
- SERP top-10 for the candidate (location + language from config).
- Take the **#1 real competitor URL** (skip aggregators: google, wikipedia, youtube, wix, hostinger, etc.).
- Pull that URL's domain ranked keywords. The **highest-volume** keyword it ranks for that matches the intent = the **real primary keyword**. Its other top keywords = **secondary keywords**.
- Caveat: huge multi-topic domains (e.g. a big marketing blog) pollute ranked-keywords; in that case trust the SERP titles + keyword_suggestions for the secondary set.

## Step 2: GSC (what already ranks)
- Top queries with impressions/clicks. Position **5-20** = quick wins (push to top 3). High impressions + low clicks = title/meta (snippet) fix.

## Step 3: DataForSEO opportunities
- keyword_overview (volume, CPC, difficulty, **search intent**), keyword_suggestions, related_keywords, competitor ranked_keywords.
- **CPC = business value.** At equal volume, prefer the higher-CPC keyword (more commercial value / better-fit audience). A modest-volume, high-CPC term often beats a high-volume cheap one.

## Step 3b: Authority-aware selection (WINNABILITY gate: pick what you can actually rank)
Volume is worthless if you can't reach page 1. Pick keywords winnable given the site's CURRENT authority, not just the biggest numbers.
- **Gauge the site's authority** from real signals: GSC impressions/positions (Step 2), domain age, and the **live SERP**: is the top 10 wall-to-wall household-name brands, or are there forums, small blogs, or thin/new pages ranking?
- **New / low-authority site (a brand-new site with little or no GSC data):** HARD-prefer **low difficulty (KD ≤ ~25-30)** + **long-tail / specific angle** + clear intent (a commercial quick-win or a focused informational question). **Do NOT target high-difficulty head terms (KD > ~35) even when volume is high**: a new site won't rank them for a long time, so log them as a "later, once authority grows" backlog instead. **Prefer a KD-9 term at vol 300 over a KD-60 term at vol 5000.**
- **Established / ranking site:** the ceiling rises with proven authority. Where GSC shows you already rank mid-page for related terms, you may reach for harder terms in those topics.
- **SERP test:** first page all big brands + you're new → wrong term for now. A forum / small blog / thin page ranking → a gap you can take.
- **Never copy a strong competitor's head keywords just because they rank for them** (their authority earned it; yours hasn't yet). Target YOUR achievable terms.

## Step 3c: Topical authority (build a cluster, don't scatter)
It's faster to become the "king" of one tight topic than to chip at a broad one. Pick the day's keyword **inside a focused cluster the site is building**, and **interlink** the new piece to its cluster (the hub/product page + sibling articles), so authority compounds. Your site config defines the clusters; stay within them rather than picking unrelated one-off topics.

## Step 4: Duplicate check, mandatory before creating (ANTI-CANNIBALIZATION)
- Cross-check the keyword against the **full existing-content list: sitemap (published) + GSC + the
  DRAFT/unpublished list** (run-protocol pre-flight step 4: your blog registry read with ALL statuses,
  or your CMS queried for `status=publish,future,draft,pending`). **The sitemap and GSC only show published
  content; a draft on the same topic is invisible there but still cannibalizes.**
- **Match by TARGET KEYWORD, not by URL.** A page can already own a keyword without that term being in
  its URL, e.g. `/features/analytics` whose `<title>` targets "social media analytics tool". Checking only
  the slug (or only the blog registry) misses it, which is exactly how a cannibalizing duplicate
  slips through. So inspect the **`<title>` / H1 / meta** of existing pages across **ALL page types
  (feature, landing, comparison, product, programmatic, AND blog)**, not just blog articles and not just
  paths: scan the sitemap URLs *and their titles*, plus the GSC queries the site already ranks for, for
  the candidate head term and near-synonyms.
- Does any page/article/**draft** already target this keyword, a near-synonym, or the same intent?
  - Yes (published) → **optimize that existing page in place** (page days) / pick another keyword
    (article days). **Never spin up a second page for a term an existing page already targets.** Cooldown applies.
  - Yes (**existing draft**) → do **not** create a competing piece. Pick a clearly different
    keyword/angle, or skip the run.
  - Genuinely different INTENT but same head term (e.g. existing product/feature page vs a new
    comparison listicle) → allowed **only if** you also retitle/re-angle so the two pages target
    **distinct** terms (one keeps the exact head term, the other shifts to a branded/feature or
    "best/compare" variant) **and** cross-link them. Default to NOT creating; split only when the live
    SERP genuinely shows two intents.
  - **HARD STOP: near-identical titles = cannibalization, even across page types.** If the candidate's
    title would *lead with the same head term* as an existing page's title (e.g. existing free tool
    "Best Time to Post on Social Media in 2026 (…)" vs a new article "Best Time to Post on Social Media
    in 2026 (…)"), do **NOT** create it; the "different intent" exception does NOT apply. **Interactive
    free tools / calculators OWN their keyword**; never duplicate one with an article on the same head
    term. Cross-linking does not make a duplicate acceptable. Optimize the existing asset, or pick a
    different long-tail (e.g. "how often to post", "best days to post", a specific audience/angle) whose
    primary term no existing page leads with.
  - No → create.

## Step 5: Competitor content analysis + differentiation angle (MANDATORY before writing)
Google and AI engines reward the page that adds the most value, not the one that copies the others.
So before writing, actually READ the top 3 ranking pages (WebFetch the real competitors from the SERP,
skipping aggregators) and produce:
1. **Coverage map**: the H2/subtopics each top page covers. The union is the baseline you must at
   least match (table-stakes), so you never ship something thinner than what already ranks.
2. **Gap + weakness list**: what they miss, get wrong, leave vague, or do badly: questions they
   never answer, missing concrete numbers/examples, outdated info, no real recommendation, walls of
   fluff, no quick answer, no FAQ. These gaps are your opportunity.
3. **Your differentiation angle (write it down)**: one clear sentence on WHY yours deserves to outrank
   them, phrased as the reader test: **"why would someone read mine instead of the current top 3?"**
   The answer must name the specific extra value you add. Pick at least 2-3 concrete value-adds, e.g.:
   - answer the questions the top pages skip (mine "People also ask" + the SERP for these),
   - more concrete specifics (real prices in the right currency, real steps, real examples, comparison
     tables) where they stay generic,
   - genuine experience / point of view (E-E-A-T): an honest recommendation, trade-offs, "when NOT to",
   - better structure for humans and AI: quick answer up top, H2-as-questions, FAQ, scannable.
4. **Never thin, never a clone.** If you cannot answer "why would someone read mine instead?" in one
   convincing sentence, do not ship it: pick a different keyword/angle. Matching the SERP is not
   enough; beating it is the bar.

## Output (the brief)
Real primary keyword + secondary keywords, intent (article/page), volume/CPC/difficulty, new-vs-optimize
decision, the **top-3 coverage map + gaps**, the **differentiation angle + the 2-3 value-adds**, and the
**verified facts** (prices/stats fetched live) the writer may use. For verified figures, fetch the
competitor/source pages; never invent numbers, and keep your site's single currency consistent
throughout. The writer then covers the baseline, fills the gaps, and leads with the angle.
