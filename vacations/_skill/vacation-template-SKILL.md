---
name: "vacation-template"
description: "Builds or updates one of Ghazal's vacation trip pages (self-contained HTML, one per trip) using her locked-in template — hero banner, stat chips, an interactive Leaflet map, and Overview / Hotel & Transportation / Itinerary / Also Worth Seeing / Book Now / Print tabs, plus trip-specific tabs like Beaches or Adventure. Use this any time Ghazal asks to plan a trip, build/update a vacation page, add a destination to her Vacations hub, or research a new place she's traveling to — even if she just names a destination and dates without saying \"template.\" Also use it when she asks to research restaurants, hikes, beaches, or logistics for a trip she's already building a page for, or to push/update a vacation page on her site. Do not use this for one-off travel questions that aren't going into a page (e.g. \"what's the weather in Rome next week\")."
---

# Vacation Template

Build or update one of Ghazal's vacation trip pages: self-contained HTML, one page per trip, sharing one structure (tab order, day-card format, output locations) while branding and trip-specific tabs flex per destination. The goal: she can open any trip page and know exactly where to find hotel info, today's weather, or the booking checklist, regardless of which trip it is.

Start every new trip from `assets/template.html` — a generalized, ready-to-fill skeleton that already encodes every rule below (CSS variable pattern, tab structure, day-card markup, checklist/localStorage wiring). Copy it rather than building from scratch, and treat it as living documentation: whenever a rule in this file changes, update the skeleton to match in the same pass, so it never drifts from what's written here.

## Ghazal's travel taste profile — the default lens for every trip

Apply this whether she hands you a full itinerary to formalize or just names a destination with almost nothing else specified (see the intake flow below).

**Lodging.** Moderate-to-high-end boutique properties, not maximum luxury. Target $700–1000/night, with $1200/night as roughly the ceiling for a default pick. A genuinely nice glamping setup in a great location is an equally valid category, not a downgrade. Views and location matter as much as the room — a well-placed mid-range property beats a generic high-end one buried inland.

**The splurge exception.** Once in a while, a single property or experience is worth blowing the budget on for its own sake — the reference point is the Junior Suite at the Blue Lagoon in Iceland (~$1800/night). The tell: it's a specific, singular experience, not just "the nicer room at a nice hotel," and it's rare — one splurge night per trip at most. If research turns up a genuine one of these, surface it as an option rather than silently booking or silently skipping it.

**Setting and pace.** Water, forests, and remote natural landscapes over cities — she chose a 45ft catamaran through the Thai islands over staying in Bangkok. Default to nature/coast/mountain settings, cap big-city time at a couple of days even when a major city is on the route, and build a deliberate balance of rest and adventure into every itinerary — no stacking high-output days back to back, and no all-lounging trip either.

**Adventure, with the kids in mind.** She travels with a 10- and 13-year-old: build in real hiking, ziplining, snorkeling, boat/water excursions, calibrated to be genuinely fun for kids that age, not an adults-only extreme itinerary. Sightseeing and culture are welcome, especially when naturally part of the place (a castle you're already walking past); she's not a big museum person, so skip forced museum stops.

**Food.** She doesn't need a nice restaurant every night — a food truck or a cash-and-carry fish counter is a win, not a compromise. Mix deliberately: a couple of proper splurges alongside plenty of casual/hidden-gem/value spots. She loves international food broadly, especially Italian, seafood, hidden-gem/local specialties, and fusion — keep the research adventurous rather than defaulting to the obvious tourist favorite in every town.

**Getting around.** Rideshare by default where it's available and reasonably priced; she rents a car in most destinations. Use public transit specifically where a private charter/taxi is disproportionately expensive relative to the public option. This profile tells you which way she's likely to lean — it doesn't replace confirming the mode with her (see the research step below).

## Output locations — every run, no exceptions

Every trip page (new or updated) gets saved to both, kept identical:

1. `~/Dropbox/Personal/Claude/Vacations/<Trip Name>/index.html` (plus any local images)
2. `~/Dropbox/Personal/Claude/site-repo/vacations/<trip-slug>/index.html` — the live site repo (git remote `github.com/gtehrani-dot/Ghazal-Website`, deployed via GitHub Pages to ghazal-tehrani.me)

After writing the site-repo copy: `git add`, commit with a short descriptive message, `git push`. A trip with multiple sub-pages (e.g. a two-leg trip) has each sub-page follow this same dual-save pattern, and the trip's hub page links to each. Before writing, check whether one location already has a newer copy than the other (someone may have edited the site-repo copy directly) — propagate the newer one rather than overwriting it with a stale copy.

Add or update this trip's card on the Vacations hub page (`site-repo/vacations/index.html`, mirrored at its Dropbox equivalent) so every trip is reachable from one place.

**If `git commit`/`git push` fails with an `index.lock` "Operation not permitted" error inside the Dropbox-mounted `site-repo`:** this is a known quirk of that mounted folder. Clone the remote fresh into a scratch directory (`git clone https://github.com/gtehrani-dot/Ghazal-Website.git`, using the credential in `site-repo/.git-credentials`). Before copying files over, reset the scratch clone to `origin/main` (other automated tasks push to this repo too, so the Dropbox working copy's git history can be behind) — then copy the updated file(s) over from the Dropbox working copy, commit (`user.name "Ghazal Moore"` / `user.email "tehrani.ghazal@gmail.com"` if identity isn't set), push, then delete the scratch clone. The Dropbox copy's working-tree file is already correct either way.

**Multi-leg hub pages carry a "Top booking needs" section**: every currently open item across all legs, deduplicated — the complete list, not a curated subset. Group it exactly like each leg's own Book Now tab (Accommodations → Transportation → Excursions → Restaurants, same label-and-rule dividers), ordered chronologically within each group. Include reminder-only items too (e.g. "no booking needed, just check the timetable closer to the date") with a lighter visual treatment rather than dropping them. A fact that spans two legs (a connecting flight, a shared transfer) gets listed once, tagged to show it spans both (e.g. "Sardinia → Como"), linking to whichever leg's Book Now tab is actionable, and sharing that item's `bookdone::shared::<id>` localStorage key so checking it off anywhere keeps it in sync. Each item links straight to the relevant leg's Book Now tab. Once every item in a group is booked, remove the item — and the whole group label if it was the last one — rather than leaving a done row; the hub only ever shows what's still open.

## The propagation rule — the one principle that governs everything below

A single fact — a hotel, restaurant, excursion, contact, or time — can live in up to five places: the itinerary day card, the map pins, the Book Now checklist, the Print tab (Key Contacts + Day-by-Day, plus its PDF), and the matching Todoist task — plus the hub's rollup for a multi-leg trip. Whenever a fact is added, changed, or removed — because Ghazal said so, research turned it up, or a Gmail sync confirmed it — update every place it lives in the same pass, not just wherever you happened to notice it. This includes removals: if a plan drops (a restaurant swapped out, a tour cancelled), it comes out of the itinerary, the map pin, the Book Now line, and both the live Print tab and `_print_source.html` — then regenerate the PDF, every time `_print_source.html` changes, as the last step of the pass. Before calling a change finished, grep the page (and the hub, if multi-leg) for the name to confirm nothing was missed — don't rely on memory of where you already updated it. The sections below reference this rule rather than re-explaining it.

## Starting a brand-new trip — the 4-question intake

When Ghazal raises a new trip idea with little more than a destination in mind, ask exactly these four things up front (via `AskUserQuestion` in Cowork, otherwise inline), then build a complete starter page from the answers without further back-and-forth:

1. **Where** — a single location, or a multi-city/multi-leg tour?
2. **General dates** — a rough month/season is enough to start.
3. **Who's going** — family (note kids' ages), a romantic trip, friends, or a mix — this drives pace, adventure level, and evenings.
4. **Anything already fixed in her mind** — a specific hotel, an activity, a region, anything already decided.

With those four answers, run the full research-and-flag process below, apply the taste profile as the default lens, and produce a complete starter page — every section at full depth, real vendors and links, not placeholders. Save both locations, push, link from the hub. Tell her plainly this is a strong starting point built from her answers plus her taste profile, not a locked plan — she should expect to swap things out. Where a real judgment call had no input from her, make the call rather than stalling, but flag the more consequential ones so she can redirect early.

## Before writing any HTML: research and flag decisions

Research first — don't template an empty page and fill it in later.

1. **Research the destination properly**: real hotels, a genuine mix of high-end and casual restaurants (always the best of each), activities, trails, beaches, real websites/booking links, actual coordinates for map pins.
2. **Decide how she's getting around, and confirm before building anything that depends on it.** Research the realistic options and form an opinionated recommendation with reasoning (e.g. "bus + ferry locally — parking is scarce and the combo reaches every stop" vs. "rent a car — the coastline stops aren't served by transit"). Lead with the recommendation via `AskUserQuestion`, never an open-ended "how do you want to get around?" Skip the question if she's already confirmed the mode. A trip can mix modes by leg — note which mode applies to which days. **The same before-you-edit discipline applies to any later logistics change with a genuine tradeoff** (cost, time, reliability) — a mid-trip route swap, a private-driver-vs-transit call, and the like. Research real numbers for each option and lay them out in chat first; don't pick one and edit the page until she's weighed in.
3. **Sequence days to minimize backtracking**: group activities by geographic cluster per day, but balance adventure days against rest/beach/slow days — a named requirement, not a nice-to-have.
4. **Surface logistical trouble spots before finalizing**: multiple viable airports/ports of entry, timed-entry or reservation-only sites, seasonal closures, one-way loops vs. backtracking routes, ferry/shuttle schedules that don't span the day. Bring these to her with a recommendation before finalizing — don't silently pick one and bury the tradeoff.
5. **Weather in day cards**: real forecast numbers within the live window (~10–16 days out). Beyond that, use seasonal climate normals, labeled with a short `<span class="wx-note">Historical</span>` tag — never a fabricated specific-looking forecast for a trip months away. Remove the tag entirely once a real forecast replaces it.

## Page structure — every trip page

### Branding
A palette, font pairing, and vibe matching the actual place — never reused from another trip. Follow `assets/template.html`'s CSS variable pattern (`--primary`, `--primary-deep`, `--accent-1/2/3`, `--ink`, `--muted`) so the rest of the template's classes stay in sync regardless of palette.

### Hero
A banner photo (or a couple, for a trip spanning visually distinct areas) plus the interactive map, always included at or immediately below the hero.

Every sub-page of a multi-leg trip needs a `← Back to trips` link at the very top, above the hero, linking to that trip's hub page (`../index.html`) — required on every leg page, confirm the href actually resolves.

Multi-destination hub pages carry the same stat-chip masthead style as their leg pages, summarizing the whole trip: flight route (origin → first stop), total nights across all legs, legs/areas covered, and base structure — same `.hero-meta`/`.meta-chip` markup as the leg pages.

The H1 always names the destination outright ("Sardinia," "Lake Como") — never just a mood tagline. A small `.hero-eyebrow` line above the H1 is a fine addition but never a substitute for naming the place in the H1 itself.

### Stat chips (every trip, always)
Directly under the H1: flight route, night count + dates, region/area covered, and base structure (one base vs. hotel-hopping).

### Timeline (multi-town/multi-base trips only)
A horizontal timeline bar — one segment per stop with icon, place name, date range, and nights — for any trip moving between two or more bases. Skip entirely for single-base trips.

### Interactive map
Leaflet + OpenStreetMap tiles (`unpkg.com/leaflet@1.9.4`), no API key. The tile layer must be the standard OSM raster tiles (`https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png`) — never a dark/night basemap variant; a dark basemap under a dark page palette renders as a solid black box with no visible geography. If a page wants a moodier map, style the surrounding chrome instead, never the tile layer.

Every pin: a numbered marker colored by category, a popup with a real photo thumbnail, the place name, and a short note. If the point has its own website, the popup includes a "Website →" link in addition to the directions link — both pieces of the popup-building logic need to reference the site variable, not just one. Every pin's popup and/or card also links to Google Maps directions (`https://www.google.com/maps/dir/?api=1&destination=<lat>,<lng>`). Per-place Google Maps links are sufficient — no bulk KML/My-Maps import.

### Tabs — always in this order
**Overview, Hotel & Transportation, Itinerary**, then 1–3 trip-specific tabs named for what they cover (a coastal trip might use Beaches + Boat Trips; a city trip might use Museums + Day Trips), then **Also Worth Seeing**, then **Book Now**, then **Print** (always last).

**Overview** — one consolidated at-a-glance table, one row per day (date, area/base, one-line focus) so the whole trip shape is scannable without reading the full Itinerary. No narrative-paragraph essay, no second set of stat cards repeating the hero chips, no "why this route" summary callout — a genuinely important heads-up belongs on the specific day card or Book Now item it concerns.

**Hotel & Transportation** — every lodging used, plus every flight and rental car, split by a `.book-group-label`-style divider into a **Lodging** group and a **Transportation** group (flights + rental car, chronological). Keep each leg's tab scoped to that leg on a multi-leg trip.

- **Lodging**: name (linked), address, check-in/out dates, confirmation number if booked, phone, a one-line why-chosen note, and — if breakfast is included — the actual service hours (e.g. "7:30–10:30 AM"), not just "included."
- **Every flight** gets its own card: route and airline (linked to manage-booking if possible), departure/arrival dates and times, confirmation number/PNR, fare class and inclusions, booked vs. priced-but-not-purchased status. **State the real baggage allowance every time** — how many checked bags the fare includes, per-traveler or per-booking, and the cost to add one — sourced from the actual confirmation/receipt, not the airline's generic policy page. If the confirmation doesn't itemize per-person vs. total, say so honestly rather than guessing.
- **Rental car**: company (linked), pickup/dropoff location and window, confirmation number, phone, car class/price/notes.
- **Public transit** (bus/ferry/train, single transfer or day-to-day): its own card every time transit is part of the plan — the current dated seasonal timetable (the actual PDF/page for the season covering her dates, not a generic search link), operator name and phone, which stop/dock, and how payment works.
- **A live multi-modal route planner** (Rome2Rio or similar) as a supplemental reference card whenever a leg strings together bus/ferry/train/walking — labeled as a reference tool, linking to the tool itself rather than a fabricated deep link.

This tab is in addition to — not instead of — the Book Now Transportation group, brief itinerary mentions, and the Print tab's full detail (see the propagation rule).

**Itinerary** — a single scrolling tab of day cards, never one tab per day. No page-top drive-time rollup — a drive time lives on that day's card, full stop. Every day card includes:

- Date + day label, and weather (temp, precipitation, wind — see the weather guidance above)
- A **meal-row** right after the weather line: three `.meal-chip` pills (breakfast/lunch/dinner) — `filled` names the actual spot, `empty` is a distinct dashed/muted style flagging a real gap rather than omitting the chip. If a meal is covered by something other than a restaurant (excursion ticket, hotel, en route with no venue), say so in the chip.
  ```html
  <div class="meal-row">
    <span class="meal-chip filled">🍳 Breakfast — Le Dolcezze Napoletane</span>
    <span class="meal-chip empty">🥪 Lunch — not planned</span>
  </div>
  ```
  ```css
  .meal-row{ display:flex; gap:8px; flex-wrap:wrap; margin:2px 0 16px; }
  .meal-chip{ font-family:<mono font>; font-size:11px; padding:5px 12px; border-radius:100px; }
  .meal-chip.filled{ background:var(--pale); color:var(--gold); }
  .meal-chip.empty{ background:transparent; color:<muted>; border:1px dashed var(--line); }
  ```
  This is a summary index for visibility — breakfast and lunch chips are informational only. Dinner chips always have a matching Book Now task, so an `empty` dinner chip automatically maps to a "find and book dinner" task in the action tab.
- **Drive time and a leave-by time** for the day's first commitment, stated concretely ("leave by 6:15am") whenever timing matters.
- On **arrival/departure days**, keep it to the practical sequence — land time, drive time, what happens once there (or checkout time and drive-to-airport on departure). The full flight chain lives on Hotel & Transportation and Print; don't re-list it here.
- Every activity, restaurant, and note **hyperlinked** to its real website. A deliberate restaurant mix — call out the can't-miss pick and the easy/affordable one, scheduled near what's already happening that day.
- Meals visually distinct: `class="meal"` on the `<li>`, tinted background block, no bullet marker (`::before{ content:''; }`).
- Every restaurant gets a rating + price-tier badge in the compact form "4.6, $" (a Google/Tripadvisor rating plus 1–3 dollar signs) next to its name — unless it's already shown elsewhere on the page, in which case just add the price tier there. Don't fabricate an unreliable rating (implausible review count, mismatched business, single-review score) — omit the badge or label it "new, $" instead.
- Split a day's `<ul class="tl-items">` into labeled `.tl-subhead` sub-sections whenever it mixes clearly distinct phases (travel vs. arrival/leisure, morning vs. evening block) — reach for this at 6+ bullets covering more than one phase; a short 2–3 bullet day doesn't need it.
- For a genuinely dense day, work out concrete clock times for the whole sequence when the building blocks are known (a landing time, a fixed duration) — write the actual sequence with real times, and flag any step that depends on a non-fixed schedule with an honest best-estimate window rather than false precision.
- **Default to the included hotel breakfast** whenever the day's leave-by time reasonably allows it (check against a ~7–7:30 AM opening) — make it the primary plan and demote an external bakery/cafe to an `alt` line. Only make an external stop primary when the departure genuinely precedes hotel service, and say so explicitly on that day's card.
- When a booked activity has a fixed meeting point and free time before it, recommend 2–3 nearby options ranked by actual drive time to that meeting point (a real Haversine check from coordinates), not proximity to some other cluster — present the closest as the easy pick and note what the others trade off.
- Alternates flagged as alternates, not equal primary options. Hotel transitions called out explicitly.
- If a rental car is part of the trip, its pickup and dropoff get their own line on the arrival and departure/checkout day cards (in addition to Book Now and Print).
- A **"Send to Google Maps"** link for the day's route: a multi-stop directions URL (`https://www.google.com/maps/dir/?api=1&origin=...&destination=...&waypoints=stop1|stop2|...`) chaining that day's stops in order.
- On a **confirmed public-transit day**: 2–3 concrete real departure times nearest the planned window for both legs (the ride out and the ride back), pulled from the current seasonal timetable, plus the last service of the day as a safety net. Use the season-appropriate timetable, not just the first one found. Skip this entirely on a drive/taxi day.
- **When a transit leg is timed for the experience itself (golden hour, sunset), not just logistics**, look up the actual sunset/golden-hour time for that location and date and pick the real scheduled departure closest to it — state why in one line (e.g. "sunset runs ~8:05–8:10pm in mid-August, so the 7:10pm ferry lands you on the water right as the light turns"). Give that as the primary pick, with an earlier real departure as the backup.
- **A departure day built around a hard flight deadline needs a full primary chain plus one complete fallback chain**, not just per-leg alternates — write out the whole sequence (each leg's real departure time) for the recommended plan, then a second complete sequence using later versions of each leg she can fall back to if the morning runs behind. State the resulting buffer before the flight for both chains so she can judge the risk herself.

**Also Worth Seeing** — attractions researched but that didn't make the day-by-day schedule.

**Book Now** — every item needing booking or advance action, across the whole trip. If public transit is confirmed anywhere on the trip, include a line linking the current seasonal schedule.

Wrap each item's label in a link to its primary booking/website; add a small inline 📞 icon-link beside the title if a phone number also exists, rather than a separate contact line — the plain-text phone/URL still lives in Print and the PDF (see the propagation rule), so nothing is lost.

Group items **Accommodations → Transportation → Excursions → Restaurants** (booking urgency order), separated by a `.book-group-label` divider matching the page's accent. Exception: a specific restaurant that's known to sell out early gets promoted to the top of the list, ahead of Accommodations, with a note on why. Within each group, open items come first (most urgent first), `done` items sink to the bottom.

No prose summary callout at the top of this tab — any flag belongs inside the specific item's own line.

Every empty meal-chip on a day card needs a matching Book Now line: name and link the identified candidate if one exists (from an `alt` suggestion or research not yet committed), or mark it research-and-book if nothing's identified yet. Two exceptions: a walk-in-only place gets no line (there's no booking action to take — the arrival strategy still belongs on the day card and in Print), and a meal during transit/travel with an obvious grab-something plan gets no nagging task either.

**Print** (always last, titled just "Print") — a minimal live tab: a title, one line of intro text, one `⬇ Download PDF` button. No inline Key Contacts or day-by-day duplication, no `window.print()` button — the PDF replaces both needs.

```html
<div id="tab-print" class="section">
  <div class="section-title">Print</div>
  <div class="section-sub">A no-internet-needed version, ready to download before you lose signal.</div>
  <a class="print-btn" href="<Trip Name>-Itinerary-Printable.pdf" target="_blank" rel="noopener">⬇ Download PDF</a>
</div>
```

The PDF is a separate minimal document (`_print_source.html`, not the live page's markup — no JS, no tab-switching assumptions), generated with `weasyprint` (`pip install weasyprint --break-system-packages` if needed) and saved as `<Trip Name>-Itinerary-Printable.pdf` alongside `index.html` in both output locations. Regenerate it every time `_print_source.html` changes, as the last step of that pass.

**`_print_source.html` is the file most likely to silently drift out of sync** — it's touched less often than `index.html`, and a string of small patches can leave it representing an old version of the plan (a flight still marked not-yet-booked after it's booked, an old stop name, a routing that's since changed) even though each patch was correct at the time it was made. Before regenerating, skim it against what's actually live on the page — if it's more than a fact or two behind, do a full rewrite of the affected sections from the current page rather than patching just the one thing you came in to fix.

Two sections, and the split matters — Key Contacts reads as "who do I call," Day-by-Day reads as "what happens when":
- **Key Contacts**: pure identity/contact info only — name, address (or nearest cross-street/landmark), phone as a `tel:` link, email if on file, website. No times, schedules, or "booked for [date]" specifics — those belong in Day-by-Day. Confirmation numbers, PNRs, and booking references live **only** here and in the PDF — never on the live-page tabs, since the site has no login. Never surface raw GPS/lat-long coordinates as visible text anywhere on the page — a street address or nearest landmark is what she actually reads; the map's own pin data still needs real lat/lng for placement, but that's invisible plumbing, not visible copy.
- **Day-by-Day**: one section per day, holding every time-specific fact — departure/arrival times, check-in windows, reservation times, transit options, timed-entry hours.
- Beaches, trails, viewpoints, and other non-bookable natural sites get no contact-block treatment (no phone, no coordinates) — a distance figure is enough, and a day with a few such options condenses to one plain-text line naming them (`Beach options: Cala Banana, Spiaggia di Portisco, or Cinque Spiagge — see Itinerary tab for details.`).

## Placeholder / TBD days — when Ghazal hasn't decided yet

When a day has 2–3 real candidates and she wants to decide closer to the date, give it a dashed-border "not yet decided" card rather than inventing a choice or leaving it blank:

```css
.tl-card.tbd{ border-style:dashed; border-color:var(--coral); background:rgba(255,122,92,0.05); }
.tl-option{ background:rgba(251,246,234,0.03); border:1px solid var(--line); border-radius:10px; padding:14px 16px; margin-bottom:10px; }
.tl-option h4{ font-family:'Fraunces',serif; font-size:16.5px; color:var(--cream); margin-bottom:4px; }
.tl-option p{ font-size:13.5px; color:rgba(251,246,234,0.75); }
```

The day's `.tl-card` gets a `tbd` class, its title names the decision ("Choose one: Cooking Class, E-Bike Tour, or Beach Club Day"), and each candidate gets its own `.tl-option` block with the same real research as a confirmed day — actual pickup points, phone numbers, links, prices. Propagate every option per the propagation rule: a map pin per option, one combined Book Now "decide + book" item (not three separate tasks), a Print entry per option. Once she picks one, remove the `tbd` class and the losing options, and fold the winner into a normal day writeup.

## Syncing bookings from Gmail

Whenever you touch a trip page — and periodically on a schedule — search her Gmail for booking confirmations matching anything named on that page (hotels, tours, boat charters, rental cars, flights, timed-entry tickets), using vendor names/domains plus terms like `confirmation`, `booking`, `reservation`, `e-ticket`. Read full threads with `get_thread`/`get_message` for anything that looks like a real confirmation, not just a quote or availability check.

Search her whole mailbox — plain `search_threads` already includes archived and sent mail; also run the same query with `in:anywhere` and `includeTrash: true` to catch a confirmation that was filed, spam-caught, or trashed by accident. For a message that exceeds the per-call token limit, either grep the saved file or retry with `messageFormat: MINIMAL` for headers/snippet only.

For each booking found, extract vendor, confirmation number, dates, room/product details, price, cancellation policy, and any special instructions, then:

- **Matches the plan exactly** (same dates, room/product, vendor) → apply automatically: mark the Book Now line `done`, move the confirmed phone/address/instructions into the relevant tab or day card, add confirmed flight details to the day card. Confirmation numbers and PNRs go only into Print/PDF, never the other live tabs (see the propagation rule).
- **Held or pending deposit/payment** is not booked — read the actual email language. Keep it open (no `done`, no "✅ Booked"), but surface the concrete next step ("pay the $X deposit to finalize") in the Book Now line and Key Contacts. Flip to `done` only on genuine confirmation.
- **Doesn't match, or is ambiguous/unresolved** → don't touch the page silently. Surface it in chat with specifics and your read, and ask before applying anything.
- **She tells you directly it's booked, with no email to confirm** (booked by phone, or a companion's confirmation went to their inbox) → take her word, mark it `done`. Fold in any real details she gave; if she gave no confirmation number, write "confirmation number not yet on file" rather than inventing one. Reconcile to harder numbers if an email surfaces later.
- **A genuinely new, unconfirmed plan surfaces** (an inquiry for something not currently in the itinerary) → don't fold it in automatically. Surface it as a heads-up and let her decide.

**Scheduled runs**: do a cheap check first — read each trip page's date range from its hero, and if every trip's end date is already past (or there are no trip pages), stop immediately with no Gmail calls. Only run the full sweep for trips still upcoming.

```css
.checklist li.done{ opacity:0.6; }
.checklist li.done .lbl{ color:var(--muted, var(--granite)); }
.checklist li.done::before{ content:'✅'; margin-right:2px; }
```

## Book Now checkboxes — genuinely clickable, not decorative

Ghazal needs to mark any Book Now item complete herself, independent of the Gmail-sync `done` state (a travel companion can book something and the confirmation never touches her email). Every `.checkbox` in a `.checklist` is a real click target, backed by `localStorage` (safe here — these are live static pages in a real browser, not an artifact preview):

```js
(function(){
  var PAGE_KEY = '<trip-or-leg-slug>';
  document.querySelectorAll('.checklist li[data-book-id]').forEach(function(li){
    var sharedId = li.getAttribute('data-book-shared-id');
    var key = sharedId ? ('bookdone::shared::' + sharedId) : ('bookdone::' + PAGE_KEY + '::' + li.getAttribute('data-book-id'));
    var stored = localStorage.getItem(key);
    if (stored === '1') li.classList.add('done');
    else if (stored === '0') li.classList.remove('done');
    var box = li.querySelector('.checkbox');
    if (box) box.addEventListener('click', function(){
      var nowDone = li.classList.toggle('done');
      localStorage.setItem(key, nowDone ? '1' : '0');
    });
  });
})();
```

Every `<li>` needs a stable `data-book-id` slug so its storage key doesn't shift between edits. An item shared across two leg pages (a connecting flight closing one leg and opening the next) uses `data-book-shared-id` instead, so checking it off anywhere keeps every listing in sync. For a page using a different Book Now markup (e.g. numbered cards instead of `.checklist li`), adapt the same idea to that pattern's own "done" indicator — the principle is what matters: every completion indicator is clickable and persists via localStorage, not just filled in by Gmail sync.

**Limit**: this localStorage checkbox only ever lives in her browser — it doesn't touch the underlying file, doesn't show on another device, and doesn't feed a hub's done-item-removal logic (which reads the file's `class="done"`, not any browser's localStorage). When she tells you directly, in chat, that something's complete, edit the file yourself — add `class="done"` to that `<li>` — rather than pointing her back to the checkbox.

## Every Book Now item is also a Todoist task

1. Find or create one Todoist project per trip page (one per leg, for a multi-leg trip), named `"<Trip Name> Trip - Actions"` matching the page's H1. Check `find-projects` first — don't create a duplicate.
2. Every open (not-yet-`done`) Book Now line becomes a task in that project: `content` = the item's label, `description` = the rest of the detail plus its contact/link, so the task is actionable without flipping back to the browser.
3. Priority is always `p1`, due date always `dueString: "today"` — every item, no exceptions. This is what makes the list read as "book these now."
4. Don't duplicate — check existing tasks (`find-tasks` by `projectId`) for a clear match before adding; clean up any real duplicates found with `delete-object`.
5. Skip anything already `done` on the page. When a Book Now line flips to `done` after its task already exists, complete the matching task in the same pass.
6. When a line's framing changes (decided → undecided, one option → several, or the reverse), find the matching task and update its wording in place — a stale task that still reads "book the cooking class" after the page moved to "decide between three options" misleads her into thinking she's committed to something she isn't.

## Updating an existing page vs. building a new one

Restructure and add missing sections/tabs only — don't rewrite or second-guess the substance of an already-researched itinerary (which restaurants, which days, which order) unless she explicitly asks to change the plan itself. If a required section is genuinely missing (no Hotel & Transportation tab yet, a day card with no weather line), research and add it. If she hasn't told you which hotel she booked, ask rather than inventing one.

## Prose style — say the fact and stop

Every line — itinerary bullets, hotel notes, Book Now descriptions — states the fact (vendor, time, price, catch) and stops. Lead with the fact or decision, not the reasoning behind it, unless the reasoning is a genuine tradeoff she has to weigh. Cut phrases that restate the obvious without adding information. One clean sentence beats two that circle the same point. Example: "Ferry + train back — scenic, affordable option. Buy at the dock/station, no advance booking needed" says everything "Not booking a private car this time — taking the ferry and train instead, which is both cheaper and part of the scenery. No advance booking needed for any of these legs; pay/tap on board or at the station" says, in half the words.

## Formatting conventions

- Three-tier font system: a serif display face for headings, a clean sans for body, a monospace for labels/eyebrows/chips — swap the specific faces per trip's vibe, keep the three tiers.
- `.meta-chip` pill row for stats, `.card-grid`/`.card` for POI grids, `.poi-actions a.site` / `a.dir` for the website/directions button pair, `.day-weather` strip per day card, `.checklist` for Book Now, `.callout` / `.callout.warn` / `.callout.muted` for trip-wide notes.
- Tabs are plain JS show/hide (`showTab(id)` toggling `.section.active`) — no framework.

## After building or updating a page

1. Save both output locations, commit + push the site-repo copy.
2. Confirm the Vacations hub links to it.
3. Give her a short summary of what changed and any open questions or decisions to flag — not a re-explanation of the whole page.
