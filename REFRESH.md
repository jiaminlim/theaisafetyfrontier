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

*RSI vs General test*: does the card measure AI working on AI/ML research itself (→ RSI), or capability in some other domain (→ General)? "Could be a precursor to RSI" is **too broad** — nearly any capability gain could be framed that way. DiG-bench (a game-discovery benchmark) is General, not RSI, for exactly this reason.

**Plain model releases are not tracked at all.** A new Gemini/Claude/Qwen/Muse version with no external validation of a specific capability result does not get a card. Only add a model-related item if it clears the Demonstrations or Research bar below.

### Risks
Three subtopics — **Malicious Use**, **Malfunctions**, **Systemic Risks** (the report's own three).

### Risk Management
Five subtopics — **Technical & Institutional Challenges**, **Risk Management Practices**, **Technical Safeguards & Monitoring**, **Open-Weight Models**, **Building Societal Resilience**.

---

## 3. The event/study marker

**Redesigned 6 Sep 2026** — the marker is no longer an emoji. Every card opens with a short horizontal line above its title, marking which side of its subcategory's split it falls on:

```html
<article class="card" data-window="7" id="...">
  <span class="mark mark-event" title="Incidents"></span>
  <h4>Title</h4>
  <p>...</p>
  <div class="tags">...</div>
</article>
```

- **`mark-event` (solid accent-coloured line) = something happened.** `title` is `Demonstrations` (Capabilities), `Incidents` (Risks), or `Implementation` (Risk Management).
- **`mark-study` (dashed neutral line) = something was studied.** `title` is `Research` in all three supergroups.

Cards are `<article class="card">`, not `<div>`, and the title is an `<h4>`, not an `<h3>` inside a `.card-title-row` — that wrapper no longer exists. The classification logic underneath (what counts as an event vs a study) is unchanged from the rules below; only the visual marker and card tag names changed.

The key is stated **once**, in the header meta row ("Reading the marks"). Never restate it in supergroup notes, subtopic notes or the footer.

**Tests:**
- *Capabilities* — a one-off striking result from a specific instance → 💥 Demonstrations; a formal, repeatable, usually named benchmark → 🧐 Research. Both require external/independent validation.
- *Risks* — did it actually happen (a real attack, breach, harm) → 💥 Incidents; is it a study/finding/investigation, including an independent investigation *of* a real incident → 🧐 Research.
- *Risk Management* — **actor-based**: 💥 Implementation means a **developer** (a company that builds/deploys AI models) has actually deployed/shipped/run the practice, **or** a government entity has turned it into an actual commitment, framework, settlement or law. Everything else is 🧐 Research — *including a government research institute's own paper, tool or benchmark*, because the institute isn't a developer and a released tool isn't a commitment. Say "developers", never "AI producers".

**Edge cases already decided — follow these:**
- A system card goes to **Risks > Malfunctions > 🧐** when its headline finding is the model misbehaving (GPT-6 Astra's chain-of-thought monitorability drop). It stays in **Risk Management > Practices > 🧐** when it's about the evaluation regime and its results (GPT-5.6 August Updates). These two sit on opposite sides deliberately.
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

### Check BOTH listings for every source, every run
Missing one has caused repeated real misses.

| Source | Listings |
| --- | --- |
| Anthropic | anthropic.com/news · anthropic.com/research |
| OpenAI | openai.com/news · openai.com/research · deploymentsafety.openai.com (system cards) |
| Google DeepMind | deepmind.google/discover/blog/ · deepmind.google/research/publications/ |
| Meta | ai.meta.com/blog · about.fb.com/news · research.meta.ai |
| UK AISI | aisi.gov.uk/blog · aisi.gov.uk/research |
| METR | metr.org/research/ · metr.org/blog/ · metr.org/notes/ |

### Known source quirks
- **OpenAI 403s WebFetch constantly** (news, research, individual `/index/` posts). Fall back to **several** targeted WebSearches — not one. A single search returns one slice of their output; a 10 Aug post sat unnoticed inside the window for weeks this way.
- **Australia's AISI** publications URL reliably times out — use WebSearch (`site:industry.gov.au` + terms) every time.
- **Meta**: research.facebook.com's RSS is dead.
- **Newsletters**: fetch the full issue and extract *every* safety-relevant story, not just the lead. If an issue says "two reports", check you carded two.
- A newsletter counts as the surfacing source even when the org it names isn't itself tracked.

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

## 6. Windows (7-day / 30-day)

Every card carries `data-window="7"` or `="30"`.

- **Anchor both windows to the pull date**: 7-day = pull − 6 days; 30-day = pull − 29 days.
- **Classify by most-recently-surfaced date** — the via/newsletter date on a two-tag card, the org date otherwise. Not the original publish date.
- **Recompute every existing card before pulling anything new**: delete cards now before the 30-day floor; roll `7`→`30` for cards now before the 7-day floor.
- A card can roll *back* into the 7-day window if a newsletter re-covers it with genuinely new information — add a second via-tag and fold the new information into the body.
- Update the `RANGES` object at the top of the `<script>` block (**not** a `#window-range` span — that id was removed in the 6 Sep redesign; the range now renders into `#snapshot-range` automatically from `RANGES[days]`. The `#pinned-range` target was removed 7 Sep 2026 along with the whole pinned mini-header — don't reintroduce a reference to it), plus the footer Window paragraph's pull date and the footer Newsletters list.

---

## 7. Card order within a subtopic

Sort by **(1) 💥 before 🧐, then (2) most-recent-surfacing date, newest first**. Re-run after adding cards.

---

## 8. Related-notes — window-aware

```html
<p class="related-note">↔ Related: <span class="rel rel-first">the <a href="#target-id">short label</a>, why it relates</span></p>
```

Sits between the body `<p>` and `.tags`. Each clause is its own `.rel` span; the first also carries `rel-first`. `setWindow()` hides clauses whose target card is hidden and hides the note entirely when none survive.

**Each clause must read standalone** — no "and its…", no "its later…" — because any clause can become the first one shown. The separator comma is generated in CSS.

**Bar for adding one:** it must bridge subtopics a reader wouldn't otherwise see together, and the link must be tight (same incident, same product line, direct causal link). Not "these happened the same week". Same-subtopic pairs don't need one.

---

## 9. Run order

1. Compute the new windows from today's pull date.
2. **Recompute every existing card's window first** — delete aged-out, roll 7→30.
3. Pull every source, both listings, plus every newsletter issue since the last run.
4. Filter to AI-safety-relevant items surfaced by a tracked source.
5. Duplicate check — fold new information into an existing card rather than creating a second card for the same story.
6. Classify, write the card, pick the 💥/🧐 marker.
7. Verify every new link resolves and every related-note anchor resolves to a real `id`.
8. Re-sort each subtopic (§7).
9. Update header window-range, `setWindow()` strings, footer Window paragraph, footer Newsletters list.
10. Validate (§10).
11. Commit and push to `main`.

---

## 10. Validation — run before every commit

**Redesigned 6 Sep 2026** — cards are now `<article class="card">` and titles are `<h4>`, not `<div class="card">`/`<h3>`. Update this script if either changes again.

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
assert c.count('class="source-btn"')==12, 'expected 12 source filter buttons'
assert 'id="filter-chip"' in c, 'filter chip missing'
print(f'OK — {n} cards')
PY
```

**Then verify placement by printing each card under its subtopic** — not just counting. A move-type edit can relocate cards between supergroups while every count still matches.

### Two bugs that have actually happened — don't repeat them
- **`html.index('data-topic="x"')` matched the CSS selector block, not the section**, back when subtopics used `data-topic`. The redesign renamed that attribute to `data-nav` on `<div class="topic" data-nav="x" id="x">` (both group and topic wrappers use `data-nav` now) — but the same class of bug applies to *any* attribute-value lookup: if the value also appears in a CSS selector earlier in the file, a plain string search finds the wrong occurrence. Anchor on the full opening tag, not just the attribute.
- **A topic's or group's span must end at `min(next sibling, its parent's actual closing tag)`**, found via depth-balanced div matching — not a naive "next occurrence of `</div>`", which over-captures nested content. Using only "next sibling, else end-of-file" over-captures the *last* item in a group and can swallow the group boundary, moving cards into the wrong supergroup.

Take a file copy before any bulk structural rewrite.

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

Palette, layout, typography and card structure are settled and have been iterated on heavily. A refresh adds and removes **cards**; it does not restyle the page.

**Redesigned 6 Sep 2026** — the page is now a light-only blurple-accent theme (ported from a Nocturne-style Design Canvas export), with a large stacked hero headline, sticky per-group and per-topic headers, and a two-column layout (sticky left-rail category label + right-hand content) inside each `<section class="group">`. There is no dark-mode variant — don't add one speculatively. **The page originally also had a sticky "pinned" mini-header that faded in at the top of the viewport once you scrolled past the hero (showing category/topic/date-range) — the user disliked it and it was removed entirely on 7 Sep 2026** (markup, CSS, and its scroll-listener JS all deleted, along with the now-dead `hero-anchor` id). Don't reintroduce it.

- All colour is a plain hex value tokenised in `:root` (this design is not OKLCH — it was ported directly from the provided source; don't convert it back without being asked).
- Cards are `<article class="card">` with an `<h4>` title and a `.mark` line indicator (see §3) — not the old `<div class="card">`/`<h3>`/`.card-mark` emoji structure.
- Never reintroduce: the "Top 3 · Press" pickup badge, the 💥/🧐 emoji markers, a `.card-title-row` wrapper, per-block `<h4>` subheadings for the event/study split, a "why it matters" sentence, or a `max-width` on the `.group-note`/`.topic-note` classes.
- If a token changes, re-check contrast (WCAG AA) — there is only one theme to check now.
- **The sticky left-rail category label (`.group-rail`, `position: sticky; top: 58px`) is a hard constraint.** A layout change that moves the group heading above the content (removing the two-column rail) was tried on 6 Sep 2026 to fix a margin-symmetry complaint and was explicitly rejected by the user ("undo THAT IS NOT WHAT I WANTED") and reverted. Don't re-attempt that fix. If margin symmetry comes up again, solve it some other way — the rail itself is not up for removal.
- `.grid` uses a fixed `repeat(3, 1fr)` (with `@media (max-width:900px)` → `repeat(2,1fr)` and `@media (max-width:560px)` → `1fr`), not `auto-fill`/`minmax` — that produced 4 columns once the grid's `max-width` cap was removed. If the grid's container width changes again, re-check the column count renders as 3 on desktop.
- **Source filter (added 6 Sep 2026)**: each entry in the Sources panel is a `<button class="source-btn" data-key="...">` (click to filter cards to that source) paired with a small `<a class="source-ext">↗</a>` that still opens the source's homepage. Filtering matches by substring against each card's `.tags a` text content (`cardMatchesSource()` in the `<script>` block) — **this means the tag text must keep containing the org's plain name** (e.g. "Anthropic · 4 Sep") or, for the two CAIS newsletters, the abbreviation used in the `data-key` ("AISN", "MLSN" — these match `via AISN #N` / `via MLSN #N` tag text, not the newsletters' full display names). Don't rename a `data-key` without checking it still matches real tag text. The filter composes with the 7/30-day window via a shared `render()` function — `setWindow()` and `toggleSourceFilter()`/`clearSourceFilter()` all call `render()` rather than duplicating the visibility logic. A dismissible `.filter-chip` in `.meta-row` shows the active filter; don't remove it without also removing the way users clear a filter without reopening the dropdown.
- **`.group-rail` is `flex: 0 0 200px` with `min-width: 0` (fixed 7 Sep 2026) — do not shrink the 200px or remove `min-width: 0`.** Flex items get an implicit "automatic minimum size" from their content; since the rail's `<h2>` contains a single unbreakable word ("Capabilities") or a long word inside a longer phrase ("Management" inside "Risk Management"), without `min-width: 0` the rail silently grows past its flex-basis by whatever amount the longest word overflows it — and since each group's heading is a different length, this made each group's body content start at a visibly different x position (first fix attempt used 150px, which stopped the drift but was still narrower than "Management" alone needs (~191px), so "Management" visibly overflowed into the gap — the user caught this after it shipped). **200px is the minimum width that fits "Management" (the longest single word among current group names, ~191px wide at max clamp font-size) with a small margin, on its own wrapped line, with zero overflow.** If a future group name introduces an even longer unbreakable word, re-measure via headless Chrome (see below) and widen the rail again rather than letting it silently drift. Don't add `white-space: nowrap` to `.group-rail h2` — that was tried and made "Risk Management" overlap the body paragraph text, since forcing the whole phrase onto one line needs ~259px. Two-line wrapping at the natural word-space is correct for a long group name; the rail width just needs to fit the longest individual word, not the whole phrase.
- **`.meta-row` uses `align-items: flex-start`, not `flex-end`** — the three stat blocks (Developments / Window / Reading the marks) have different content heights, so bottom-aligning them put the labels at different heights. Top-aligning keeps all three labels on one line. If a fourth stat block is ever added here, keep flex-start.
- **`.scroll-mask`** is a fixed, background-colored strip pinned to the very top of the viewport (`height: 58px`, disabled at the `<640px` breakpoint) — it exists solely to mask the gap between the viewport top and the sticky `.topic-head`'s `top: 46px` offset, which would otherwise let scrolled-past content visibly peek through that gap now that the old pinned mini-header (which used to cover it) is gone. Don't delete it without checking that gap is covered some other way.
- **Headless Chrome is available in this environment** for visual verification — `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome --headless --disable-gpu --screenshot=out.png --window-size=W,H file:///path/to/index.html` renders a real screenshot; append a `<script>` block to a scratch copy of the file and use `--dump-dom` to read back `getBoundingClientRect()` values as text for precise layout checks (e.g. confirming two elements' left edges actually match) rather than guessing from CSS alone. Use a scratch copy, never the real `index.html`, for any injected debug script.
