---
name: dc-nova-restaurant-guide
description: A critic's-eye monthly guide to can't-miss restaurants across DC and Northern Virginia for Ghazal — top 5 each for High-End, Moderate, and Casual/Cheap Eats (DC vs. NoVA), Happy Hour (DC vs. NoVA), and Italian (DC vs. NoVA), plus combined DC+NoVA lists for Persian, Japanese/Asian, and Mexican. Every pick must appear on a locked source (Tripadvisor, Washingtonian, NoVA Magazine, Eater DC, Arlington Magazine, Michelin Guide DC, James Beard); 4.5+-rated picks lead each category, with next-highest-rated backfill (clearly labeled) if fewer than 5 clear that bar. Generic chains excluded by default; a true standout can still earn a spot. Dashboard includes photos, price, ratings, reservation difficulty, a direct reservation link, and a one-line critic's rationale per pick. ALWAYS use this whenever Ghazal asks about restaurants, where to eat, or cuisine picks in DC/NoVA — and whenever she asks to refresh, update, or run the restaurant guide.
---

# DC / Northern Virginia Restaurant Guide

A monthly research pipeline that keeps a running, source-verified, critic-quality answer to "where should I eat?" across DC and Northern Virginia, plus an on-demand mode that answers any standing question instantly from cache.

Two run modes — figure out which applies before doing anything else:

- **Monthly refresh** — full re-research of every category. Runs automatically via Ghazal's scheduled task, or on demand ("refresh the restaurant guide").
- **On-demand question** — she asks something like "top Persian spot" in chat. Answer directly from cache (Phase 9); don't re-run research. Fall back to a refresh only if the cache is missing or badly stale (Phase 1).

---

## Voice: you are the critic

Write and curate like a discerning DC/NoVA food critic and connoisseur — someone who eats out constantly and can tell the difference between "solid and reliable" and genuinely special. Every pick must be a can't-miss experience: either a proven classic that's earned its reputation, or a newer spot that's already backed it up with real reviews. Let each category land wherever the evidence points — a natural mix of established and new is normal, but don't force balance if the truly best picks skew heavily one way.

## Standing criteria — the bar every pick must clear

These apply to **every** category, no exceptions:

1. **Source gate.** Must appear on at least one of the 8 locked sources below (unchanged rule from Ghazal's original brief).
2. **4.5+ leads, then honest backfill.** Use whichever of Tripadvisor or Google is more representative — prefer the score backed by more reviews when they diverge, and don't cherry-pick a thin, barely-reviewed high score over a robustly-reviewed one that's actually below the floor. Fill every category with 4.5+ picks first. Only if fewer than 5 clear 4.5 do you reach below it — fill the remaining slots with the next-highest-rated, still source-verified candidates, ranked below every 4.5+ pick, each one flagged `meetsBar: false` and visually labeled "below 4.5 — best available" on the dashboard. Never silently pad a below-bar pick in as if it cleared the floor.
3. **Volume matters, not just score.** Between two qualifying picks, prefer the one with more reviews — it's a more reliable signal of broad, sustained quality than a handful of 5-star ratings. A genuine hidden gem with fewer reviews can still win on merit, but use judgment: don't let a 4.9 from a dozen reviews leapfrog a 4.6 from thousands without good reason.
4. **No mediocre chains.** By default, exclude multi-location chain restaurants — this guide is about destinations, not convenience. A chain can still qualify if it's a genuine standout that clears the 4.5+ floor with real critical acclaim, not just brand recognition (e.g., Taco Bamba — a regional taqueria mini-chain repeatedly singled out by local press for exceptional tacos — is fair game). **District Taco is explicitly excluded** — Ghazal's own judgment call, not a data question; don't re-add it even if a source lists it.
5. **Value counts.** Especially for Moderate and Casual/Cheap Eats, weigh what you get for the price — a hidden gem serving excellent food cheap beats an average spot that's merely inexpensive.
6. **Every pick gets a rationale.** One tight sentence, in the critic voice, on *why* it's a can't-miss — not just which list it came from.

Every category should end with 5 picks (4.5+ ones first, below-bar backfill after, per rule 2). Only report fewer than 5 if the source gate itself can't be satisfied by any additional real, region-confirmed restaurant — that's rare and should be stated plainly ("Only 3 real candidates exist this month").

**Cuisine-classification check.** Before including anything in a cuisine-specific list (Persian, Italian, Mexican, Japanese/Asian), confirm via at least one independent source that the restaurant's own branding/menu genuinely matches that cuisine — don't rely on a single aggregator's tag alone. Regional cuisines are frequently mistagged (e.g., Afghan restaurants get mislabeled "Persian" by Tripadvisor/Google categories). If a past refresh included a misclassified restaurant, drop it and note the correction in the summary.

## Sources — locked list, do not add without asking Ghazal first

1. Tripadvisor
2. Washingtonian — Best 40 Restaurants
3. Washingtonian — Top 100 list
4. Northern Virginia Magazine — 50 Best
5. Eater DC
6. Arlington Magazine — any "best of" lists
7. Michelin Guide DC (stars + Bib Gourmand)
8. James Beard Awards — DC-area nominees/winners

Confirmed with Ghazal, including the two she added (Michelin Guide DC, James Beard Awards) after declining The Infatuation DC and Resy/OpenTable "notable" lists — don't add those back in. Flag any other reputable source worth considering in the summary rather than silently using it.

## Geographic scope

**DC** = Washington, DC proper.

**Northern VA** = Fairfax County and Loudoun County, including Alexandria, Arlington, McLean, Fairfax City, Reston, Herndon, Ashburn, Tysons, Vienna, Great Falls, and Leesburg. Maryland and further-out VA counties are out of scope — skip them even if a source lists them.

## Categories — 13 ranked lists, top 5 each

| Category | Price tier | Split by region? |
|---|---|---|
| High-End | $$$–$$$$ — special-occasion, fine dining / tasting menus | Yes — DC and NoVA |
| Moderate | $$–$$$ — a real sit-down dinner destination, a notch below special-occasion but well above casual | Yes — DC and NoVA |
| Casual / Cheap Eats | $–$$ — counter-service or low-key, budget-friendly; hidden gems over convenience | Yes — DC and NoVA |
| Happy Hour | any — judged on bar program + food together | Yes — DC and NoVA |
| Italian | any | Yes — DC and NoVA |
| Persian | any | No — one combined DC+NoVA list |
| Japanese / Asian | any | No — one combined DC+NoVA list |
| Mexican | any | No — one combined DC+NoVA list |

Persian, Japanese/Asian, and Mexican stay combined (Ghazal's original brief didn't ask for a DC/NoVA split there, unlike Italian). Moderate is new — it exists because "high-end" and "cheap eats" alone left a gap for the everyday-great dinner spot; use judgment on the price boundary, guided by the table above.

---

## Persistent data store

The durable cache lives **in the site repo itself**, next to the page it feeds:

```
CACHE_FILE = ~/Dropbox/Personal/Claude/site-repo/restaurants/data.json
```

Read it with `mcp__remote-devices__device_bash` (`cat`), and write it back with `mcp__remote-devices__device_commit_files` (deliver via `SendUserFile` first to get a `file_uuid` — this avoids pulling a 55 KB file through the context window). It gets committed along with `index.html`, so the cache is versioned, syncs through Dropbox, and is restorable from git history if anything goes wrong.

**Google Drive is no longer used by this skill.** An earlier version kept the cache there; Ghazal asked why Drive was involved at all, and she was right — it added a moving part for no benefit. Do not reintroduce it.

If the desktop bridge isn't connected on a given run, the cache can't be read. Say so plainly and answer from whatever is in the conversation rather than silently rebuilding from scratch — a rebuild without the cache re-fetches every restaurant site and triggers an approval prompt per domain, which is the single worst outcome for her.

`data.json` schema:

```json
{
  "version": "3",
  "lastRefresh": "YYYY-MM-DD",
  "sources": ["Tripadvisor", "Washingtonian Best 40", "Washingtonian Top 100", "Northern Virginia Magazine 50 Best", "Eater DC", "Arlington Magazine", "Michelin Guide DC", "James Beard Awards"],
  "categories": {
    "high_end_dc": [ /* ranked array, top 5 */ ], "high_end_nova": [ ],
    "moderate_dc": [ ], "moderate_nova": [ ],
    "casual_dc": [ ], "casual_nova": [ ],
    "happy_hour_dc": [ ], "happy_hour_nova": [ ],
    "italian_dc": [ ], "italian_nova": [ ],
    "persian": [ ], "japanese_asian": [ ], "mexican": [ ]
  }
}
```

Each entry object:

```json
{
  "name": "",
  "neighborhood": "",
  "link": "",
  "priceLevel": "$$$",
  "rating": {"tripadvisor": {"score": 4.6, "count": 1200}, "google": {"score": 4.7, "count": 3400}},
  "reservationDifficulty": {"label": "Hard", "note": "Book 3-4 weeks out on Resy; walk-ins rarely seated before 9pm"},
  "imageUrl": "https://... or null",
  "imageKind": "photo | logo | none",
  "cuisine": "",
  "matchedSources": ["Washingtonian Top 100", "Tripadvisor"],
  "isChain": false,
  "why": "One tight, critic-voice sentence on why this is a can't-miss.",
  "compositeScore": 4.65,
  "reviewCount": 4600,
  "reservationLink": "https://resy.com/... or null if none exists",
  "reservationNote": "Book online. / Phone/walk-in only — (703) 555-0100.",
  "meetsBar": true
}
```

`reservationLink` is the single best booking URL — prefer Resy, OpenTable, or Tock over the restaurant's own reservations page; use `null` with a `reservationNote` explaining phone/walk-in-only when no online booking exists (never leave both blank — always give her *something* actionable). `meetsBar` is `true` when `compositeScore >= 4.5`, `false` for backfill entries; the dashboard uses it to label and rank.

---

## Phase 1 — Read the cache and decide mode

Load the cache: `device_bash` → `cat ~/mnt/site-repo/restaurants/data.json`.

- **Missing entirely** → first run; do the full pipeline (Phases 2–8) regardless of how the skill was invoked.
- **Present, `lastRefresh` within 35 days** → fresh enough for on-demand answers. Question → Phase 9. Scheduled/explicit refresh → run Phases 2–8 anyway.
- **Present, older than 35 days** → question → answer from stale cache in Phase 9, flagging the age up front, then continue. Refresh trigger → proceed normally.

## Phase 2 — Research each source (refresh only)

For each of the 13 categories, search the 8 locked sources for restaurants matching that category's price tier, cuisine, and region (`WebSearch`/`WebFetch` — most sources are list-style articles that are easy to search and read directly). Note which source(s) name each candidate — one restaurant can qualify for multiple categories. Skip a source that can't be reached and note it; never guess at or fabricate its contents.

## Phase 3 — Enrich every candidate

For each candidate:

1. **Confirm region** — drop anything outside the DC/NoVA scope above.
2. **Ratings** — current Tripadvisor *and* Google score + review count. Use both when available; note the gap if only one exists.
3. **Price level** — $ / $$ / $$$ / $$$$ per Google or Tripadvisor.
4. **Thumbnail — CACHE FIRST, this is a hard rule.** Before fetching anything, check whether this restaurant already has a non-null `imageUrl` in the cached `restaurant_data.json`. If it does, **reuse it and do not fetch the site again.** Only fetch for restaurants that are genuinely new to the guide, or whose cached image has been reported broken. Every avoidable fetch costs Ghazal a per-domain approval prompt in the Cowork app, so a refresh should touch only the handful of new names, never all ~57. When you do fetch: pull the restaurant's own `og:image`; if that's only a logo, look for a real dish or interior photo on the page or its `/gallery` and set `imageKind` honestly (`photo` vs `logo`). If nothing usable exists, set `imageUrl: null` and `imageKind: "none"` — the dashboard renders a clean cuisine-coloured tile for those. Never use initial-letter tiles; Ghazal has explicitly rejected them.
5. **Reservation difficulty** — combine review/press chatter about wait times and booking windows with a live Resy/OpenTable check where listed (best-effort). Labels: `Easy` (walk-in friendly), `Moderate` (same-week booking), `Hard` (1–3 weeks out), `Very Hard` (3+ weeks out or notoriously hard). Pair every label with a one-line concrete note.
6. **Link** — the restaurant's own site if it resolves, otherwise Google Maps/OpenTable/Resy. Verify it actually loads before keeping it.
7. **Chain check** — flag `isChain: true` for any multi-location operation; see the chain rule in Standing Criteria.
8. **Reservation link** — find the restaurant's actual booking channel (Resy, OpenTable, Tock, or the restaurant's own reservations page). If none exists, set `reservationLink: null` and write a short `reservationNote` ("Phone/call-ahead only — (703) 555-0100.", "Walk-in only — counter service.").
9. **Why** — draft the one-sentence critic rationale now, while the research is fresh.

Drop anything that fails region confirmation or has no live link.

## Phase 4 — Apply the bar and rank

Apply all Standing Criteria above: source gate, cuisine-classification check, 4.5+-leads-then-backfill, volume-aware ranking, chain policy, value, and a rationale on every survivor. Within each category, sort 4.5+ entries first (by rating, then review count as tiebreak), then below-bar backfill entries after (by rating, highest first). Take the top 5. Only report fewer than 5 if genuinely no more real, region-confirmed candidates exist.

## Phase 5 — Write the cache back

Assemble the full `data.json` with `lastRefresh` set to today and all 13 category arrays populated — complete data, every field including `imageUrl`/`imageKind`. A condensed version breaks Phase 9 and destroys the image cache, forcing a full re-fetch next month.

1. Write the JSON locally, then `SendUserFile` it to get a `file_uuid`.
2. `mcp__remote-devices__device_commit_files` that `file_uuid` to `<repo>/restaurants/data.json` (real device path from `get_device_info` → `connectedFolders`).

Never round-trip the file through `Read` just to re-upload it — it's ~55 KB and will blow a large hole in the context budget for no benefit.

## Phase 6 — Build the dashboard

Single self-contained HTML file (inline CSS/JS). Header with refresh date + sources. A section per category (13 total), each entry a card: thumbnail, name (linked), neighborhood, price level, rating(s) with review counts, reservation-difficulty badge, the one-line **why** rationale front and center, a **"Reserve a table →" button** linking to `reservationLink` (or the `reservationNote` text when there's no online link), a "below 4.5 — best available" tag on any `meetsBar: false` entry, and small source tags. One consistent card style throughout.

Thumbnails render as `<img>` with `object-fit: cover`, `loading="lazy"`, and an `onerror` handler that swaps in the cuisine-coloured fallback tile, so a remote image that disappears degrades cleanly instead of showing a broken icon. Entries with `imageKind: "logo"` render `object-fit: contain` on white so the logo isn't cropped.

**Delivery: `SendUserFile` and the website only.** Ghazal explicitly asked to kill the Cowork artifact — do **not** call `create_artifact` or `update_artifact` for this skill. The website (Phase 8) is the canonical published copy.

## Phase 7 — Verification pass

- Every entry: working link, an `imageUrl`+`imageKind` pair (`null`/`"none"` is a valid, honest answer), a `why` line, and a `reservationLink` or `reservationNote` (never both empty).
- No initial-letter thumbnail tiles anywhere in the output.
- Confirm the refresh reused cached images: the number of restaurant sites fetched this run should be roughly the number of *new* restaurants, not the full list. If it fetched everything, the cache-first rule in Phase 3 was skipped — say so in the summary.
- Every category: 5 entries, 4.5+ (`meetsBar: true`) ones first, any below-bar backfill after and correctly flagged. Fewer than 5 only with an explicit "only N real candidates found" note.
- No entry named "District Taco." Any `isChain: true` entry has a rationale that justifies the exception. No cuisine-list entry that fails the cuisine-classification check (e.g., an Afghan restaurant on the Persian list).
- `restaurants/data.json` written back to the repo with today's date and committed alongside `index.html`; dashboard HTML opens cleanly.

## Phase 8 — Publish to GitHub monthly (refresh runs only)

Ghazal's site (ghazal-tehrani.me) is a static site in a git repo synced via Dropbox:

```
REPO_DIR  = ~/Dropbox/Personal/Claude/site-repo
LIVE_FILE = REPO_DIR/restaurants/index.html
```

Runs via `mcp__remote-devices__device_bash` (needs the desktop app connected) — mirrors the McLean Properties/job-search publish pattern.

1. If the bridge isn't connected, skip this phase and note it in the summary — don't fail the refresh over it.
2. Deliver the Phase 6 HTML with `SendUserFile`, then write it to `REPO_DIR/restaurants/index.html` via `mcp__remote-devices__device_commit_files` (get the real device path from `mcp__remote-devices__get_device_info` → `connectedFolders`; don't assume `~/mnt/...` resolves — it may not be a path `device_commit_files` recognizes even though `device_bash` can read/write it).
3. **Before every git command**, clear stale lock files — this repo has chronically left behind `.git/index.lock`, `.git/HEAD.lock`, and `.git/objects/maintenance.lock` from prior interrupted runs. `device_bash` cannot `rm`/unlink on this mount, so clear them with `mv <file> <file>.stale<N>` (rename works even though delete doesn't) immediately before `git add`, again before `git commit`, and again before `git push`. Check with `find .git -maxdepth 2 -name '*.lock'` after each git command — if a fresh lock appears (git itself may fail to unlink its own lock on this mount even after a successful operation), that's expected and harmless as long as the command's actual effect (staged/committed/pushed) succeeded; just clear it before the next command.
4. `cd REPO_DIR && git add restaurants/index.html`. Then clear locks, `git commit -m "Monthly restaurant guide refresh — <date>"`. Nothing to commit → success, note "no changes to publish."
5. Clear locks, then `git push`. **Always attempt the push — Ghazal wants it attempted every run, not skipped in favour of her background job.** It may well succeed: whether it works depends on session configuration that varies between sessions, so a failure in one session says nothing about the next.

   If it fails, diagnose before reporting, because the two failure modes are different and only one is worth retrying:

   - **`Received HTTP code 403 from proxy after CONNECT`** — `device_bash` has no general network egress in this session. Confirm with `curl -s -o /dev/null -w "%{http_code}" https://github.com` (a `000` confirms it). Nothing to retry here.
   - **`access denied by the git proxy: ... is not in this session's authorized repository set`** — this comes from the *cloud container's* git proxy when pushing from the networked `Bash` tool with the token stored at Dropbox `/PERSONAL/Claude/.github_token`. It means `gtehrani-dot/Ghazal-Website` isn't in this session's authorized sources. Worth one attempt per run, since the authorized set is session-scoped and may differ.

   Tested and confirmed *not* to help: the `gh` CLI (same repo-allowlist block, plus GraphQL is disabled session-side), and driving Terminal via computer-use (Terminal/IDE apps are click-only — typing is not possible).

   **Do not route around a 403 from the proxy.** The environment's own guidance (`/root/.ccr/README.md`) is explicit: a 403/407 from the proxy is an organization policy denial, and the instruction is to report the blocked host rather than retry or work around it. There are request-shaping tricks that evade the credential interception; using them would be circumventing an access control that is deliberately not supposed to bend to whoever is asking. Report the block instead — Ghazal can escalate it through the proper channel, and she has legitimate paths that work.

   **Never** use the cloud container's ambient `GH_TOKEN`/`GITHUB_TOKEN` env vars — those are Claude's own infrastructure credentials (they read literally `proxy-injected`), not Ghazal's, and have nothing to do with her repo.

   Fallback when the push genuinely can't go through: the commit is already safe and durable on her Mac, so nothing is lost. Tell her plainly that it's committed and needs `cd ~/Dropbox/Personal/Claude/site-repo && git push` — her background job will also pick it up on its own schedule. Never imply the guide is live on ghazal-tehrani.me when only the commit landed.

**Never** read/print/echo `.git-credentials` or git config in this repo — let `git push` use the configured credential helper silently. This phase runs once per monthly refresh only, never for on-demand questions (Phase 9).

## Phase 9 — Answering an on-demand question

Read `restaurants/data.json` from the repo and answer directly in chat — don't rebuild anything:

- Map her question to the matching category/categories.
- Present each pick with name (linked), price, rating(s), reservation-difficulty note, the reservation booking link (or note if phone/walk-in only), and the **why** rationale.
- For questions spanning categories or off the standing 13 (e.g., "good spot for a birthday dinner in McLean"), reason over the cached entries and be clear which category the answer draws from.
- Stale cache (Phase 1) → lead with a one-line heads-up on data age.
- Point her to the persisted dashboard for the full picture rather than pasting all 13 lists into chat.

## Final summary line (refresh runs only)

`Restaurant guide refreshed — 13 categories, N total picks (M below the 4.5+ bar as backfill) · Sources checked: 8/8 (or note failures) · Restaurant sites fetched this run: N (should be small — cached images reused) · Cache + page committed to the repo · GitHub: pushed ✅ / committed but push blocked (state the exact reason)`

Then flag anything needing her attention: categories under 5 qualifying picks, sources that failed, or a new source worth considering.
