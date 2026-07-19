# Writing Rules: the shared blocking standard for every site

This is a **blocking checklist**. No article or page is written or delivered unless it passes
every UNIVERSAL rule plus your site's own VOICE rules (defined once in the template at the bottom).

## UNIVERSAL: apply to every site, every article and page

### The bar test (TOP rule, every article and page)
- Write as if **you were explaining it to a friend at a bar**: direct, human, in plain words
  anyone understands, as if a real person wrote it and not an AI. Address the reader directly.
- **Translate or drop every jargon term.** A non-technical business owner must understand on the
  first read. Banned without a plain-language explanation: "stack", "headless", "back office",
  "deliverable", "deployment", "responsive" (say "adapts to phones"), etc.
- Have real, concrete opinions. Use everyday examples. If it reads like a corporate brochure, rewrite it.
- (Your site's own rhythm still applies: a premium brand can keep flowing sentences and a polished feel, but human and clear, never stiff or AI-polished.)

### Anti-AI-slop (hard fails)
- **Two-layer anti-slop: a constraint pass DURING writing, a humanizer pass AFTER.** Keep this file's
  constraint rules + banned lists active while you DRAFT, so the first draft comes out clean at the
  source. Then run the humanizer pass below on the finished draft to catch whatever slipped through.
  Generation-time prevention plus post-hoc cleanup.
- **MANDATORY humanizer pass before ANY article or page ships, every site.** After the draft is
  written, audit the body text against the known signs of AI writing (Wikipedia's "Signs of AI writing"
  guide is the reference set: em/en dashes, staccato/short choppy sentences, rule-of-three, AI
  vocabulary, negative parallelisms, filler, vague attributions, etc.) and rewrite every hit. Run it as
  its own draft -> audit -> final loop. This is not optional and not a substitute for the rules below;
  it is the first net, the rules below are the second.
- **House overrides on top of the humanizer pass (we are STRICTER than its defaults):**
  - **NO short sentences at all.** A typical humanizer voice keeps some "short punchy sentences" for
    rhythm; we do NOT. Every sentence reads like flowing human prose. After the humanizer pass, run the
    short-sentence scan and fix every hit: split frontmatter off, then flag any sentence under 6 words
    (`python3` split on `.!?`), merge each into its neighbour. Zero sentences under 6 words ship.
  - **Em/en dashes stay a hard zero** (stricter than the standard humanizer tolerance).
  - Keep the banned-word and banned-phrase lists below as a final grep gate.
- **Zero em dashes or en dashes anywhere.** Use a regular dash with spaces ` - `, a comma, or a period.
- **No banned words:** delve, tapestry, landscape, robust, seamless, cutting-edge, groundbreaking,
  transformative, unprecedented, pivotal, leverage, harness, unlock, unleash, navigate, foster,
  elevate, embark, furthermore, moreover, additionally, consequently, notably, compelling,
  innovative, dynamic, utilize, comprehensive, paramount, meticulous, game-changer, streamline,
  scalable, crucial, remarkable, profound, multifaceted, nuanced, facilitate, endeavor, resonate,
  bolster, underscore, illuminate, empower, supercharge, skyrocket, shed light on, cultivate.
  (If your site publishes in another language, ban that language's equivalents of the same
  empty modern/innovative/elegant/robust family too.)
- **No banned phrases:** "here's the thing", "let's dive in", "it's worth noting", "in conclusion",
  "at the end of the day", "in today's world", "when it comes to", "that being said", "a testament to",
  "move the needle", "hits different/home/hard", "the answer? …", "the result? …", "my take:", "hot take:",
  "the hard part", "the hard part isn't …", "the part that catches … off guard", "here's the part".
- **No throat-clearing filler before the point.** Never open a sentence with a vague set-up that delays the
  real content: "the hard part is …", "what trips most sites up …", "the part that catches you off guard …",
  "here's the thing", "the truth is …", "what most people miss …". These all read as AI slop. Lead with the
  substance directly: if you can delete the opening clause and the paragraph still makes sense, delete it.
  Test every sentence: would a real person actually say this out loud? ("get tripped up", "at the end of the
  day", "it's worth noting": no human says these. Cut them.)
- **No "Not X. It's Y." pivots. No "No X. No Y. Just Z." triplets. No three-adjective triads** ("fast, reliable, secure").
- **No rhetorical-question transitions** ("The root cause? …"). **No transition-word abuse** (furthermore/moreover): use "but", "and", "also".
- **Contractions mandatory** (EN: it's, don't, can't, won't, doesn't, never the formal form).
- **Vary sentence length** for human rhythm. **No standalone sentence under 6 words** as a fragment.
- **Quotation punctuation = logical/British**: commas and periods go OUTSIDE the quotes.

### Lists & tables (STRICT: prose is the default; lists are the exception)
- **Write in flowing paragraphs by default.** A list is allowed ONLY when the content is a genuine
  parallel enumeration that genuinely reads worse as prose: ordered steps, a true short checklist, or a
  few discrete options. If a "list" item is a full explanatory sentence, it must be a **paragraph**, not a bullet.
- **Hard caps per article:** at most **2-3 lists total**, never more than **one list per ~2-3 H2 sections**,
  each list **3-6 items max**, short items (≤ ~1 line). **No** two lists back-to-back, **no** nested lists.
- **NEVER let the article degrade into bullets, especially the second half / the end.** The closing
  sections must be as fully written (real prose, real sentences) as the intro. Bulleting the tail because
  "writing prose is tedious" is a hard fail (it reads as lazy AI). Re-write any such section as paragraphs.
- **Tables** only for a true 2-D comparison (price by type, feature matrix) and **NOT in every article**:
  most articles need ZERO tables; add one only when a real comparison genuinely reads better as a grid than
  as prose. Always write a plain **markdown table** (`| col | col |`), never hand-rolled HTML: the site build
  should turn a markdown table into a fully responsive one that **reflows to stacked cards on mobile**. **NEVER
  use `overflow-x` / horizontal scroll on a table, EVER** (readers hate scrolling tables), and never a
  forced `min-width` that causes it: on phones a table must stack into cards (one card per row, each cell
  labelled with its column header), it must never scroll sideways. Never use a table or list as a dumping
  ground to avoid prose.
- Self-review check: if removing every bullet/table and the article would feel *gutted*, the lists were
  load-bearing (OK). If it would simply read as cleaner prose, **convert them back to paragraphs**.

### Where the checks run
- The jargon / em-dash / banned-word / bar-test checks run on **every rendered surface**, not just
  the body: title, meta description, **FAQ questions and answers**, image alts, and any structured
  content in the registry. A clean body with jargon in the FAQ still fails.

### Accuracy (hard fail if violated)
- Every price/stat/date/product claim must be **verified via web fetch during the run**. Never from memory. This is doubly true for **AI model names, versions and pricing**: your training data is stale and WILL be wrong (it may confidently name a model version that has since been superseded), so fetch each vendor's current lineup and pricing live (OpenAI, Anthropic, Google, xAI), use only the verified names, and always prefer the very latest model. If you cannot verify a model or price live, do not name it.
- Use exact figures, never rounded/embellished. Distinguish self-reported vs independently verified. If unverifiable, omit.

### Article structure
- **Quick answer at the very top**: the opening directly answers the title in 2-4 sentences, self-contained (GEO / AI citation).
- **H2 headings phrased as questions** when possible ("How…", "How much…", "Why…", "Should you…").
- **First sentence after each H2 directly answers that heading** (AI extracts the first 1-2 sentences).
- **Generate ALL images of the post: the featured/hero image INCLUDED, plus one under EVERY H2.**
  Never skip the featured image. **Every single H2 gets its own image, with NO exceptions except the
  final conclusion / CTA section** (that one stays imageless). A comparison-table section, an "example"
  section, a "why it matters" section all still get their own image: do NOT leave middle sections bare.
  Before shipping, count H2s vs images and confirm `images == H2_count - 1` (only the conclusion lacks
  one). Each image is a unique concept (never reuse one). Alt text = the section's keyword (featured =
  primary keyword).
- **Image PLACEMENT:** each section image sits at the TOP of its section, right after the H2 and its
  first (answer) paragraph, never at the end of the section. Shape:
  `## heading` → first paragraph → `![alt](src)` → rest of the section. **Never re-embed the featured
  image in the body** (the page layout renders it). Use plain markdown `![alt](src)` for images unless
  your renderer requires otherwise (some MDX setups have no import support, so an `import`/`<Image>`
  component in the content file crashes the page). If a site already has an established placement
  convention across its existing articles, keep that convention consistent rather than mixing two styles.
  Formats: AVIF in-page, WebP for the OG/hero copy.
- **DIRECT-SUBJECT, NEVER A METAPHOR (HARD GATE, every site: overrides any "symbolism / metaphor / concept-pattern"
  wording anywhere else).** Every image, featured and section, must depict the article's **ACTUAL, LITERAL subject**:
  the real thing the piece is about: the named product, brand, tool, UI / screen, place, or concrete on-topic
  object or scene. **When the topic names a brand or product (Google, ChatGPT, a CMS, a SaaS tool, a specific
  feature), SHOW IT directly**: its real logo / branding, its real colours, a realistic UI or screen of it;
  brand logos, screens / UI and short clean real text ARE allowed when they ARE the subject. **BANNED:
  abstract / metaphor / symbolic "stand-in" imagery**: any object that only *represents* the idea by analogy
  (an hourglass for time, a padlock standing in vaguely for "security", a brass telegraph key for "a signal",
  vials, a maze, a chess piece). **Test: a stranger glancing must see the article's REAL subject, not a symbol
  of it.** If a prompt's subject connects to the topic only by analogy, it is WRONG: rewrite it to the literal
  subject.
- **FULL-BLEED, ZERO MARGIN (HARD GATE, every site).** The subject / UI must FILL THE ENTIRE FRAME edge to
  edge, touching all four edges. NEVER a floating window / card / object on an empty studio background with
  margin around it: no drop-shadow gap, no reflection beneath, no padding, no border, no empty negative-space
  framing. It must read like a real full-frame screenshot or photo cropped to 16:9. Every prompt states the
  subject fills the whole frame with no margin / no background around it (for UI: "no floating window, no card
  on a background, no drop shadow, no reflection"). The floating-card-with-margin look is rejected on sight.
- **Images follow `image-prompts.md` (the canonical spec, every site):** pick a real composition (product /
  UI shot, macro detail, real object, real scene), map THIS article's topic to its **LITERAL subject (never
  symbolism)**, then write all 15 elements. Read it before generating any image.
- **Every image-generation prompt must be ULTRA-detailed: 500-800 words each (HARD GATE, every
  site).** A vague 1-2 sentence or ~350-word prompt is a FAIL and gives flat, low-quality images.
  Each prompt must specify, concretely: the exact subject/scene; named real objects, materials and
  textures (brushed metal, frosted glass, walnut, marble, cotton); exact colors per element; camera
  spec (full-frame, 50mm at f/2.0, angle, eye level); a full lighting setup (direction, quality, time
  of day, fill); spatial relationships and composition (rule of thirds, focal point, leading lines,
  negative space, depth/bokeh); atmosphere/mood; "4K, ultra sharp"; and explicit bans (no text, no
  letters/numbers, no logos/watermarks, no readable UI, unless your site's image policy allows them). The
  article's topic must be guessable from the image alone, and each image in a post must use a DIFFERENT
  visual approach (vary conceptual still-life vs scene), never the same pattern repeated.
- **FEATURED image = a UNIQUE concept per article, NEVER a recurring template (HARD GATE, every site).**
  The #1 tell of automated content is every post sharing the same hero scene. **BANNED default: the
  top-down "workspace flat-lay" (desk + open laptop/screen + coffee mug + sticky notes + notebook +
  pen).** Do not reach for it. There is NO need for a screen-on-a-desk: derive the featured concept
  DIRECTLY from THIS article's specific subject and pick a composition no recent post used. Rotate
  deliberately across distinct archetypes, e.g. the real product / UI of the subject; a single REAL on-topic
  object in macro (the actual thing the article is about, never a symbol of it); an environmental/location
  shot; close-up hands performing the article's actual action; an overhead of a NON-desk surface; a
  textural/material study of the real subject.
  Two articles must never read as "the same photo with different props". Even when the topic is
  posts/analytics/screens, find a fresh angle (a phone in a hand, a printed chart pinned to a wall, a
  macro of one element on glass) rather than the desk scene again.
- **Site image aesthetic (define ONE, keep it consistent within the site):** define ONE fixed brand
  palette and aesthetic for your site and reuse it in every image prompt. Example: warm near-black
  background (#141414) + one muted copper accent (#B87333, never bright orange), photoreal cinematic
  still life, NO people, NO text. Decide once whether your site allows people and readable text in
  images, then never vary it. The palette is the constant; the concept still changes every article
  (see `image-prompts.md`).
- **Image filenames must be short and keyword-rich** (descriptive slugs), never generic
  (`hero.avif`, `section-1.avif`). Use the section's keyword, e.g. `website-redesign-cost.avif`,
  `website-redesign-steps.avif`. Featured image filename = the primary keyword slug.
- **No "Conclusion" / "Introduction" / "In summary" headings.** Use a substantive synthesis/action heading instead.
- **FAQ: 4-8 natural-language questions**, primary keyword in ≥2, + FAQPage JSON-LD.
- **Word count: 2,500-3,000** for articles and pages. 2,500 is the floor to rank; **~3,000 is a hard cap**: a longer wall of text just doesn't get read and dilutes the page. Aim ~2,500-2,800, completeness over length, every section earning its place. When OPTIMIZING an existing page, adding content (FAQ, sections) must not push it past ~3,000; if it would, tighten weaker parts instead of just piling on.
- Lists and tables are fine for scannability (within the strict caps above).

### SEO / metadata / schema
- **Title length (no ellipsis):** the **RENDERED** `<title>` must stay ≤ ~60 characters / ~575px so Google shows it in full. **Check the site's title template first**: if your layout (or the framework) appends the brand (e.g. a template like `%s · Your Brand`), your title value must **NOT** include the brand again, or you get a double brand ("… | Your Brand · Your Brand") and truncation. Count the rendered title (your value + whatever the template adds), primary keyword first.
- **Meta description ≤ ~150-155 chars** (~920px) so it isn't truncated, primary keyword in the first 20 words. (Google may still rewrite/truncate either field on some queries; we control length, not Google's choice; staying within these limits maximizes full display.)
- **Titles must be varied, never repetitive across the blog (HARD GATE).** Do NOT reuse the same
  filler word in every title (e.g. "guide" / "the complete guide" on every article).
  Each title gets a distinct angle (a year, a benefit, a question, a number, "without getting it wrong",
  "steps/pricing", a specific audience, etc.). **Before saving, fetch the live sitemap and scan
  the existing titles: if the word "guide" (or any other filler) already appears a lot, your new title
  must NOT use it.** As a rule of thumb, no more than ~1 title in 4 across the blog should contain
  "guide". When in doubt, drop the filler word entirely and lead with the concrete angle.
- 4-8 keywords, primary first. **Canonical** always. **hreflang** for bilingual sites.
- Exactly one `<h1>` with the primary keyword.
- **OG + Twitter image** at page level (WebP). JSON-LD per type: BlogPosting + Breadcrumb + FAQPage (articles); Service/SoftwareApplication + FAQ + Breadcrumb (pages).

### Internal linking
- **3-5+ internal links** per article, descriptive keyword anchors, never "click here"/"learn more".
- **Relevance-gated** (link only when topically additive) and **cooldown-aware** (don't re-edit pages changed in the last ~1 month).
- Bilingual sites: **same-locale only** (FR→/fr, EN→/en), targets published in that locale, anchors in the matching language.
- **HUB / INDEX RULE (MANDATORY, every site).** When the new page belongs to a
  listing/hub/index page (e.g. `/compare`, `/free-tools`, `/for`, `/use-cases`, a category or
  comparison hub), you MUST **add the new page to that hub's listing** so it is reachable from the index,
  not just deep-linked. If the hub is a manual array/list in code, append an entry (name + URL +
  one-line description) in the same shape as the existing ones; if it is auto-generated from the
  filesystem, nothing to do. **Verification gate before "done": fetch the hub URL and confirm it now
  lists the new page** (e.g. your `/compare` hub must show every `/compare/*` page). A comparison or
  tool page that is not in its hub is treated as an incomplete run.

## SITE VOICE (define once per site; overrides the universal rules where they differ)

Define your site's voice ONCE in its config and apply it to every piece. Fill in every row:

| Define | What to decide |
|---|---|
| Persona | Who is speaking: a named founder (direct, witty), a premium studio (calm expert), a product authority |
| Sentence rhythm / tone | Punchy and bursty, OR long flowing sentences with one idea well carried, OR clear and practical: pick one and keep it |
| Brand-mention policy | How often the brand appears (e.g. 5-8x per page, paired with the keyword at least twice) OR an explicit no-stuffing rule |
| Language | The site's language(s); for bilingual sites, which language is written first |
| CTAs | The exact CTA set (sign-up, pricing, contact, "start a project") with the exact offer wording |
| Currency | ONE currency, used consistently across every piece on the site |
| Forbidden claims | Offer wording you must never use (e.g. never call a paid trial a "free plan") and any claim your product can't back |

## SELF-REVIEW GATE (MANDATORY: run on the finished draft BEFORE saving / committing / posting)

After writing, do **ONE** editor pass over the draft against this checklist. This is a bounded review,
**NOT a loop**. Concretely:
- **First, confirm the humanizer pass actually ran on the body and the no-short-sentence scan is clean:**
  0 sentences under 6 words, 0 em/en dashes, no banned word/phrase hits. If any of those failed, run the
  humanizer pass + the short-sentence merge now, before anything else. This is the anti-slop gate.
- Read the draft once and note ONLY the specific items that actually fail (most drafts pass most items).
- Make **targeted fixes** to those specific items: edit the offending sentence/title/link/block. Do NOT
  rewrite the whole article, and do NOT regenerate images or re-run keyword research to "satisfy" the gate
  (only regenerate an asset if it is genuinely missing or broken).
- Then re-check ONLY what you changed. **Cap: one corrective pass.** If, after that single pass, something
  still doesn't pass and would need a full rewrite, do NOT loop: leave it as a draft and write the issue
  in the status line for human review (a human reviews before publish anyway).
- Token discipline: the gate is a quick read-and-fix, not a regeneration cycle. If everything passes on the
  first read, save immediately. Record "self-review passed" (or the noted issue) in the status line.

**Value & keyword (the ranking core):**
- [ ] Keyword came from real keyword data (verified volume, live SERP, intent): no guessing.
- [ ] keyword-method **Step 5** was done: the draft covers the baseline the top-3 cover AND fills their
      gaps, and the **differentiation angle is actually delivered** (the piece is clearly more useful than
      the current #1-#3, not a thinner clone). The reader test must still hold on the finished draft:
      **"why would someone read mine instead of the current top 3?"** If you can't answer it in one
      convincing sentence pointing at real content in the draft, it is NOT ready.
- [ ] Every fact/number/price is real and verifiable (fetched, not invented); correct currency for the site.

**Writing quality:**
- [ ] **2,500-3,000 words, never over ~3,000** (bilingual sites: EACH language version in range). Quick answer up top. H2s phrased as questions.
- [ ] 0 em-dash. No AI-slop triplets, no dramatic sentence fragments, no banned words. Contractions, flows.
- [ ] FAQ present (+ FAQ schema/accordion per your site's pattern). First sentence after each H2 answers the heading.
- [ ] Site voice respected (exact offer wording, no forbidden claims, the site's rhythm, no brand-stuffing beyond your policy, no stat-stacking).
- [ ] Title varied (not formulaic "guide"), primary keyword first; **rendered `<title>` ≤ ~60 chars with NO duplicated brand** (account for the layout title template); meta description ≤ ~150-155 chars, keyword in the first 20 words.

**Assets & technical:**
- [ ] All images present (featured + one per H2), ultra-detailed prompts used, keyword-slug filenames,
      **first content image alt = exact focus keyword**, AVIF in-page + WebP OG, your site aesthetic.
- [ ] 3-5 internal links, relevance-gated, descriptive anchors (bilingual: same-locale). No invented/broken URLs.
- [ ] Format valid for the site: page-builder sites = valid blocks with UNIQUE ids + a FAQ accordion (no
      raw `<h2>/<p>` HTML); code-based sites = typecheck + build clean; JSON-LD + canonical present.
- [ ] Reads human (target well under ~30% on AI-detection tools): if a paragraph sounds robotic, rewrite it.

If anything is borderline, fix it rather than ship it. Quality is constant only because this gate runs
every time; it is not optional.
