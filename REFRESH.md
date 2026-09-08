# The AI Safety Frontier — daily refresh manual

Operating manual for the scheduled agent that refreshes this site. `index.html` is the whole site: one self-contained file, no build step. Commit to `main` and GitHub Pages redeploys automatically.

**Standing instruction from the owner: prioritise thoroughness over speed.** A lead that looks thin is a reason to dig, not to skip.

---

## 1. What the page is

A personal AI-safety news dashboard organised on the *International AI Safety Report* (Bengio-chaired, 2026 edition) taxonomy. Three boxed supergroups, each with subtopics, each holding cards. Every card is one development from a tracked source.

---

## 2. Taxonomy and where a card goes

### Capabilities
Two subtopics, in this on-page order:
1. **General Capabilities** (`data-topic="general-capabilities"`) — capability outside AI's own R&D: mathematics, science, security, general reasoning.
2. **Recursive Self-Improvement** (`data-topic="rsi"`) — AI accelerating AI research itself: automated alignment research, ML replication, measured acceleration in AI-relevant R&D, agent-learning-speed scaling.

*RSI vs General test*: does the card measure AI working on AI/ML research itself (→ RSI), or capability in some other domain (→ General)? "Could be a precursor to RSI" is **too broad** — nearly any capability gain could be framed that way.

**Plain model releases are not tracked at all.** A new Gemini/Claude/Qwen/Muse version with no external validation of a specific capability result does not get a card. Only add a model-related item if it clears the Demonstrations or Research bar below.

### Risks
Three subtopics — **Malicious Use**, **Malfunctions**, **Systemic Risks** (the report's own three).

### Risk Management
Five subtopics — **Technical & Institutional Challenges**, **Risk Management Practices**, **Technical Safeguards & Monitoring**, **Open-Weight Models**, **Building Societal Resilience**.

---

## 3. The event/study marker

Every card opens with a short horizontal line above its title, marking which side of its subcategory's split it falls on:

```html
<article class="card" data-window="7" data-date="YYYY-MM-DD" id="...">
  <span class="mark mark-event" title="Incidents"></span>
  <h4>Title</h4>
  <p>...</p>
  <div class="tags">...</div>
</article>
```

- **`mark-event` (solid accent-coloured line) = something happened.** `title` is `Demonstrations` (Capabilities), `Incidents` (Risks), or `Implementation` (Risk Management).
- **`mark-study` (dashed neutral line) = something was studied.** `title` is `Research` in all three supergroups.

Cards are `<article class="card">`, titles are `<h4>`. The key is stated **once**, in the header meta row ("Reading the marks"). Never restate it in supergroup notes, subtopic notes or the footer.

**Tests:**
- *Capabilities* — a one-off striking result from a specific instance → 💥 Demonstrations; a formal, repeatable, usually named benchmark → 🧐 Research. Both require external/independent validation.
- *Risks* — did it actually happen (a real attack, breach, harm) → 💥 Incidents; is it a study/finding/investigation, including an independent investigation *of* a real incident → 🧐 Research.
- *Risk Management* — **actor-based**: 💥 Implementation means a **developer** (a company that builds/deploys AI models) has actually deployed/shipped/run the practice, **or** a government entity has turned it into an actual commitment, framework, settlement or law. Everything else is 🧐 Research — *including a government research institute's own paper, tool or benchmark*, because the institute isn't a developer and a released tool isn't a commitment. Say "developers", never "AI producers".

**Edge cases already decided — follow these:**
- A system card goes to **Risks > Malfunctions > 🧐** when its headline finding is the model misbehaving. It stays in **Risk Management > Practices > 🧐** when it's about the evaluation regime and its results. These two sit on opposite sides deliberately.
- An open letter or diplomatic communiqué is **💥 Implementation**, not Research — it is a real institutional act, and "Research" means an actual study, not "anything that isn't a programme".
- A purely descriptive societal-effects finding is **Systemic Risks > 🧐**, not Challenges. Challenges is specifically the evidence dilemma / evaluation gap / competitive dynamics.
- Resilience requires someone *actively building capacity or responding*. Not merely subject matter that touches society.
- Open-Weight Models is for the safety tension of strippable safeguards, evidenced by real red-teaming/eval work — not any open-weight release.

---

## 4. Tracked sources — 12

**AI Labs**: Anthropic · OpenAI · Google DeepMind · Meta
**Research Organisations**: UK AI Security Institute · US CAISI · Australia's AI Safety Institute · METR
**AI Commentary** (newsletters): Import AI · The Batch · CAIS AI Safety Newsletter (AISN #N) · CAIS ML Safety Newsletter (MLSN #N)

**A story only qualifies if a tracked source actually surfaced it.** However well-corroborated something is in the general press, if no tracked source carried it, it does not go on the board. This is firm.

### Check ALL listings for every source, every run

| Source | Listings |
| --- | --- |
| Anthropic | anthropic.com/news · anthropic.com/research · anthropic.com/transparency/voluntary-commitments (Risk Reports) |
| OpenAI | openai.com/news · openai.com/research · deploymentsafety.openai.com (system cards) · openai.com/global-affairs/ and openaiglobalaffairs.substack.com (governance/policy) |
| Google DeepMind | deepmind.google/discover/blog/ · deepmind.google/research/publications/ |
| Meta | ai.meta.com/blog · about.fb.com/news · research.meta.ai |
| UK AISI | aisi.gov.uk/blog · aisi.gov.uk/research |
| METR | metr.org/research/ · metr.org/blog/ · metr.org/notes/ |

### Known source quirks
- **OpenAI 403s WebFetch constantly** (news, research, individual `/index/` posts). Fall back to **several** targeted WebSearches — not one; a single search only returns one slice of their output.
- **Australia's AISI** publications URL reliably times out — use WebSearch (`site:industry.gov.au` + terms) every time.
- **Meta**: research.facebook.com's RSS is dead.
- **Newsletters**: fetch the full issue and extract *every* safety-relevant story, not just the lead. If an issue says "two reports", check you carded two.
- **Checking whether a new newsletter issue exists: always try a direct WebFetch of the newsletter's own archive/RSS page FIRST, before falling back to WebSearch.** WebSearch lags real publish time by hours while the page gets crawled/indexed — a newsletter published minutes (or hours) before this refresh runs can still come back empty on WebSearch even though it's already live, causing a false "nothing new" conclusion (this has actually happened: Import AI 472 was live ~90 minutes before a refresh's WebSearch check and still wasn't found). A direct fetch of the source itself has no such lag. Try these first, every run:
  - Import AI: `https://importai.substack.com/archive` (or `/feed` for the raw RSS)
  - The Batch: `https://www.deeplearning.ai/the-batch`
  - CAIS AI Safety Newsletter: `https://newsletter.safe.ai/archive` (or `/feed`)
  - CAIS ML Safety Newsletter: `https://newsletter.mlsafety.org/archive` (or `/feed`)
  Only fall back to several varied WebSearch queries if the direct fetch is blocked (egress-restricted sandboxes have blocked these before) or errors out. If forced onto WebSearch alone for a newsletter that publishes on a predictable day (Import AI: Sundays) and today is at or past that day with nothing found, don't treat that as confident proof nothing published — note the uncertainty explicitly in the run's summary rather than silently concluding "nothing new," since it may just not be indexed yet.
- A newsletter counts as the surfacing source even when the org it names isn't itself tracked.
- **Periodic standalone reports (Anthropic's Risk Report, OpenAI's Global Affairs output) don't live on the main news/research listings — checking those more often won't catch them**, since they're never indexed there at all. Run a standing WebSearch every refresh for `"Anthropic risk report" <current year>` and `"OpenAI" "global affairs" OR "frontier governance" <current year>` regardless of what the listings show. If a similar standalone-report pattern turns up for another tracked org, add it to the table above.

---

## 5. Card conventions

- **Body: exactly one sentence**, factual, never copied from the source. No "why it matters" second sentence.
- **Lead with the finding, not the method.** "[Org] found [result] … based on [method]" — not "[Org] tested/analysed/ran …". Exception: process-announcement cards where the method *is* the news.
- **Active voice, named actor.** Name the actual institution(s) for academic papers — check the PDF title page; "researchers" alone is not enough.
- **Title: verbatim from the primary source.** If you swap the link, **swap the title and org date with it** — they are one unit.
- **Tags — one-tag card** (primary source only): `Org · date`, linking to the specific article.
- **Tags — two-tag card** (newsletter-surfaced): primary first (`Org · date`, the original document, with the *primary's own* publish date), then `via Newsletter #N · date`. If the primary genuinely has no date, leave it dateless rather than guessing.
- No count badges, no cadence tags, no per-card feed URLs.
- The `.subtopic-note` / `.supergroup-note` have no `max-width` — don't add one.

---

## 6. Windows (New / 7-day / 30-day)

Every card carries **both** `data-window="7"` or `="30"` **and** `data-date="YYYY-MM-DD"`. Both encode the same underlying fact — the card's most-recently-surfaced date — but serve different jobs: `data-window` buckets a card into the 7/30-day tiers, `data-date` is the exact ISO date used by the "New" toggle (see §12) to find whichever cards share the single most recent date on the page. **A new or edited card must always carry both, kept consistent with each other.**

- **Anchor both windows to the pull date**: 7-day = pull − 6 days; 30-day = pull − 29 days.
- **Classify by most-recently-surfaced date** — the via/newsletter date on a two-tag card, the org date otherwise (for a card like "OpenAI · 6 Aug (updated 19 Aug)", use the updated date). Not the original publish date. This same date is what goes in `data-date` (currently all 2026 — mind the year if that ever changes).
- **Recompute every existing card before pulling anything new**: delete cards now before the 30-day floor; roll `7`→`30` for cards now before the 7-day floor. `data-date` never changes once set — only `data-window` rolls.
- A card can roll *back* into the 7-day window if a newsletter re-covers it with genuinely new information — add a second via-tag, fold the new information into the body, and update `data-date` to the new surfacing date too.
- Update the `RANGES` object at the top of the `<script>` block, plus the footer Window paragraph's pull date and the footer Newsletters list. The "New" toggle's range text is computed automatically from the cards' `data-date` values — nothing to update there by hand.

---

## 7. Card order within a subtopic

Sort by **(1) 💥 before 🧐, then (2) most-recent-surfacing date, newest first**. Re-run after adding cards.

---

## 8. Related-notes — window-aware

```html
<p class="related-note">↔ Related: <span class="rel rel-first">the <a href="#target-id">short label</a>, why it relates</span></p>
```

Sits between the body `<p>` and `.tags`. Each clause is its own `.rel` span; the first also carries `rel-first`. `render()` hides clauses whose target card is hidden and hides the note entirely when none survive.

**Each clause must read standalone** — no "and its…", no "its later…" — because any clause can become the first one shown. The separator comma is generated in CSS.

**Bar for adding one:** it must bridge subtopics a reader wouldn't otherwise see together, and the link must be tight (same incident, same product line, direct causal link). Not "these happened the same week". Same-subtopic pairs don't need one.

---

## 9. Run order

1. Compute the new windows from today's pull date.
2. **Recompute every existing card's window first** — delete aged-out, roll 7→30.
3. Pull every source, both listings, plus every newsletter issue since the last run, plus the standing standalone-report WebSearches (§4).
4. Filter to AI-safety-relevant items surfaced by a tracked source.
5. Duplicate check — fold new information into an existing card rather than creating a second card for the same story.
6. Classify, write the card, pick the 💥/🧐 marker, and set both `data-window` and `data-date` (§6).
7. Verify every new link resolves and every related-note anchor resolves to a real `id`.
8. Re-sort each subtopic (§7).
9. Update header window-range, `setWindow()` strings, footer Window paragraph, footer Newsletters list.
10. Validate (§10).
11. Commit and push to `main`.

---

## 10. Validation — run before every commit

```bash
python3 - <<'PY'
import re
c=open('index.html').read()
for tag in ['div','p','span','section','article','h4']:
    o=len(re.findall(rf'<{tag}[ >]',c)); cl=c.count(f'</{tag}>')
    assert o==cl, f'{tag} imbalance: {o} open vs {cl} close'
ids=set(re.findall(r'id="([^"]+)"',c))
dead=[x for x in re.findall(r'href="#([^"]+)"',c) if x not in ids]
assert not dead, f'dead anchors: {dead}'
t=re.findall(r'<h4>([^<]*)</h4>',c)
dup={x for x in t if t.count(x)>1}
assert not dup, f'duplicate titles: {dup}'
n=c.count('<article class="card"')
assert n==c.count('class="mark '), 'every card needs exactly one marker'
assert n==len(re.findall(r'data-date="\d{4}-\d{2}-\d{2}"',c)), 'every card needs a well-formed data-date'
assert c.count('class="source-btn"')==12, 'expected 12 source filter buttons'
assert 'id="filter-chip"' in c, 'filter chip missing'
print(f'OK — {n} cards')
PY
```

**Then verify placement by printing each card under its subtopic** — not just counting. A move-type edit can relocate cards between supergroups while every count still matches.

### Bulk-edit safety
- **When searching for a section by an attribute value (e.g. `data-nav="x"`), anchor on the full opening tag, not just the attribute string** — the same value can also appear in a CSS selector earlier in the file, and a plain string search will find that instead.
- **A topic's or group's span must end at `min(next sibling, its parent's actual closing tag)`**, found via depth-balanced div matching — not a naive "next occurrence of `</div>`", which over-captures nested content and can swallow the group boundary, moving cards into the wrong supergroup.
- Take a file copy before any bulk structural rewrite.
- **Screenshot the page after every refresh**, not just after a layout-specific fix — a `--window-size=1400,700` screenshot of the top of the page is enough to catch a stray visible element that validation alone won't (see the headless Chrome note in §12).

---

## 11. Deploy

```bash
git add index.html
git commit -m "Refresh: <pull date>"
git push origin main
```

GitHub Pages serves `main` at the repo root. `CNAME` holds the custom domain — don't delete it; Pages rewrites it if the domain is changed in settings.

---

## 12. Design constraints — do not change these

Palette, layout, typography and card structure are settled. A refresh adds and removes **cards**; it does not restyle the page.

The page is a light-only blurple-accent theme with a large stacked hero headline, sticky per-group and per-topic headers, and a two-column layout (sticky left-rail category label + right-hand content) inside each `<section class="group">`. There is no dark-mode variant and no pinned/fixed mini-header — don't add either speculatively.

- All colour is a plain hex value tokenised in `:root` (not OKLCH) — don't convert it without being asked.
- Cards are `<article class="card">` with an `<h4>` title and a `.mark` line indicator (§3).
- Never reintroduce: a "Top 3 · Press" pickup badge, 💥/🧐 emoji markers, a `.card-title-row` wrapper, per-block subheadings for the event/study split, a "why it matters" sentence, or a `max-width` on `.group-note`/`.topic-note`.
- If a token changes, re-check contrast (WCAG AA).
- **The sticky left-rail category label (`.group-rail`, `position: sticky; top: 58px`) is a hard constraint — do not restructure it into a heading-above-content layout.** A past attempt to do this (to fix a margin-symmetry complaint) was explicitly rejected by the owner. If margin symmetry comes up again, solve it another way.
- **`.group-rail` is `flex: 0 0 200px` with `min-width: 0` — do not shrink the width or remove `min-width: 0`.** Flex items get an implicit minimum size from their content; without `min-width: 0`, the rail silently grows past its flex-basis to fit whatever the longest unbroken word in a group heading needs, which then makes different groups' body content start at different x positions. 200px is the minimum that fits "Management" (the longest word among current group names) on its own wrapped line with zero overflow. If a future group name introduces a longer unbroken word, re-measure via headless Chrome and widen the rail. Don't add `white-space: nowrap` to `.group-rail h2` to force one line — a two-word group name needs far more width than 200px on one line and will overlap the body text; two-line wrapping at the natural word-space is correct.
- `.grid` uses a fixed `grid-template-columns: repeat(3, 1fr)` (with `@media (max-width:900px)` → `repeat(2,1fr)` and `@media (max-width:560px)` → `1fr`), not `auto-fill`/`minmax`. If the grid's container width changes, re-check the column count still renders as 3 on desktop.
- **Source filter**: each entry in the Sources panel is a `<button class="source-btn" data-key="...">` (click to filter cards to that source) paired with a small `<a class="source-ext">↗</a>` that still opens the source's homepage. Filtering matches by substring against each card's `.tags a` text content (`cardMatchesSource()`) — **the tag text must keep containing the org's plain name** (e.g. "Anthropic · 4 Sep") or, for the two CAIS newsletters, the abbreviation used in the `data-key` ("AISN", "MLSN" — these match `via AISN #N`/`via MLSN #N` tag text, not the newsletters' full display names). Don't rename a `data-key` without checking it still matches real tag text. The filter composes with the 7/30/New window via a shared `render()` function. A dismissible `.filter-chip` in `.meta-row` shows the active filter.
- **`.meta-row` uses `align-items: flex-start`, not `flex-end`** — the stat blocks have different content heights, so bottom-aligning them puts the labels at different heights. Keep flex-start if a fourth stat block is ever added.
- **`.scroll-mask`** is a fixed, background-colored strip pinned to the viewport top (`height: 58px`, disabled at `<640px`) that masks the gap between the viewport top and the sticky `.topic-head`'s `top: 46px` offset — without it, scrolled-past content visibly peeks through that gap. Don't delete it without covering that gap some other way.
- **"New" window toggle**: a third button in `.window-toggle` alongside 7-day/30-day, `data-days="latest"`, labelled "New". It filters to whichever cards share the maximum `data-date` found on the page (`latestDateISO()` scans every `.card[data-date]` and takes the max ISO string — no hardcoded date to maintain). This can legitimately show many cards, one card, or none — that's correct behavior. `.window-toggle button` padding/font-size is reduced under the `<640px` breakpoint to prevent the three-button row clipping on narrow screens — re-check 320px before widening the label or padding.
- **Hero highlight bar**: `.headline .swatch` (the accent box behind "Frontier") grows in from `scaleX(0)` to `scaleX(1)` on load via the `swatch-grow` keyframe animation, guarded by `@media (prefers-reduced-motion: no-preference)`. Don't remove the reduced-motion guard.
- **Footer** (`footer.notes`) opens with `<h2 class="notes-heading">Notes from the Author</h2>`. The Categories row states that Risks/Risk Management come directly from the report, the capability split and event/study mark are the site's own, and cards are "placed by judgment using an LLM; human review is conducted periodically" — keep that disclosure accurate. The Refresh row states the daily midnight AEST cadence.
- **Headless Chrome is available in this environment** for visual verification — `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome --headless --disable-gpu --screenshot=out.png --window-size=W,H file:///path/to/index.html` renders a real screenshot; append a `<script>` block to a **scratch copy** of the file (never the real `index.html`) and use `--dump-dom` to read back `getBoundingClientRect()` values as text for precise layout checks rather than guessing from CSS alone.
- **`--window-size` below ~500px is silently floored to 500px in this environment's Chrome build — plain `--window-size=375,800` etc. does NOT actually render at 375px, so it cannot be trusted for any check below ~500px** (confirmed: 450→500, 320→500, 600→600, 1600→1600). This produced false-confidence test results earlier — a real bug (see the `.related-note` fix below) went undetected this way. **For any check that specifically needs a true mobile width (<500px), drive Chrome via the DevTools Protocol instead**: launch with `--remote-debugging-port=9333 "--remote-allow-origins=*"`, then use `Emulation.setDeviceMetricsOverride({width, height, deviceScaleFactor:2, mobile:true, screenWidth:width, screenHeight:height})` over the CDP websocket before navigating (Python's `websocket-client` + `requests` packages work for this; `pip3 install websocket-client requests` if not already present). Checks at ≥500px (including the existing 640/900px breakpoints) are unaffected and plain `--window-size` remains fine for those.
- **Page must always keep `<meta name="viewport" content="width=device-width, initial-scale=1">`** (added 8 Sep 2026, right after `<meta charset="UTF-8">`) — without it, every real mobile browser ignores all `@media` breakpoints entirely and renders the page at a fake desktop width, zoomed out. This was missing for the site's entire life until caught. Don't remove it.
- **`.related-note` must not use `display:flex`.** It did until 8 Sep 2026: when a two-clause note wrapped onto a second line, the `.rel:not(.rel-first)::before` comma separator rendered orphaned at the start of the new line instead of attached to the previous clause's end. The `.rel` spans are inline, so plain (non-flex) text flow wraps them correctly like normal prose. If related-note styling is ever touched again, keep it flex-free.
- **Any element toggled via the `hidden` HTML attribute needs an explicit `.your-class[hidden] { display: none; }` rule if that class also sets its own `display` value** — a class selector's `display` beats the browser's default `[hidden]` rule, so the element stays visible even while `hidden` is set. Check this on every toggled/dismissible element.
- **Mobile-only overrides live inside `@media (max-width: 640px)` — never change these values outside that block, and never remove the mobile overrides to "simplify."** Current ones: `h1.display` uses `clamp(56px, 14vw, 72px)` on mobile instead of the desktop clamp (the desktop clamp floors to a flat 42px below ~447px viewport width, which reads as too small for a hero on any phone); `.meta-row` switches to `flex-direction: column` with `.sources { margin-left: 0; }` so Developments/Window/Reading-the-marks/Sources stack as one consistently left-aligned list instead of wrapping unpredictably (Sources' desktop `margin-left: auto` would otherwise strand it right-aligned on its own wrapped line); `.group-rail` stays `position: sticky` at `top: 0` (with its own background and higher z-index) instead of going `static`, and `.topic-head`'s mobile `top` is `45px` to sit directly beneath it without overlapping — this makes the group name (Capabilities/Risks/Risk Management) stick throughout its whole section on mobile too, exactly like it already does on desktop, with the topic name stacking beneath it during its own subsection. If `.group-rail`'s mobile height ever changes (longer group name, different font-size), re-measure it via `getBoundingClientRect()` and update `.topic-head`'s mobile `top` to match.
- **`Page.captureScreenshot` with `captureBeyondViewport: true` (the "full-page screenshot" mode used elsewhere in this doc) cannot show sticky elements in their stuck state** — it composites the whole document as if the viewport were infinitely tall, so nothing ever actually needs to "stick." To verify sticky behavior, you must actually scroll first: set normal (non-beyond-viewport) device metrics, navigate, run `window.scrollTo(0, Y)` via `Runtime.evaluate`, wait briefly, then capture *without* `captureBeyondViewport`. A full-page screenshot showing a sticky element in its resting position proves nothing about whether it actually sticks during a real scroll.
