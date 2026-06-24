# Pattern Specification Language — Draft v3

> Addresses inline feedback on application test + v2. All changes grounded in the DoorDash application test.

---

## Revisions from v2

| xac comment | Change in v3 |
|---|---|
| `use_when`/`not_when` related to `rationale`? | Clarified in section map: they're different roles — selection conditions vs. design reasoning |
| Array notation for repeating elements | `[]` suffix notation: `section[]`, `content_card[]` — maps to programming array mental model |
| Structure `rationale` per Borchers/Tidwell? | `rationale` now has labeled sub-parts: **Problem** / **Forces** / **Resolution** (prose, not YAML keys) |
| `footer` assumed to be `bottom_nav` | `footer.type` enum: `bottom_nav \| persistent_search \| action_bar \| combined` |
| Interaction behaviors missing | New optional `interactions` block: `component tap → TargetScreen` |
| Problem / Forces / Solution | Collapsed into structured `rationale` (prose with labels, not three sections) |
| Sketch | Dropped |

---

## Section Map (v3)

| Section | Purpose | Primary reader | In LLM prompt? |
|---|---|---|---|
| `name`, `id`, `category` | Catalog indexing and retrieval | System | Yes |
| `use_when`, `not_when`, `app_genres` | **Selection conditions** — when to retrieve this pattern | System + Designer | Yes |
| `layout` | Generative skeleton — component hierarchy for code generation | LLM | Yes |
| `interactions` | Interaction flows — what happens when the user acts | LLM | Yes |
| `constraints` (must / must_not / should) | Design rules the LLM must respect | LLM | Yes |
| `content` | Content types that fill the components | LLM | Yes |
| `rationale` | **Design reasoning** — why this layout resolves the tension | Designer | No (UI only) |
| `variants` | Alternative layouts for adjacent contexts | Designer + System | No |
| `examples` | Real apps that instantiate this pattern | Designer | No |

### `use_when` vs `rationale` — they are different

`use_when` answers **when** — a structured condition the retrieval system uses to decide whether to surface this pattern given the designer's current context. It is machine-readable and acts like an `if` clause.

`rationale` answers **why** — design reasoning that explains the tensions this layout resolves and how it resolves them. It is human-readable prose and is only shown in the designer UI (not injected into the LLM prompt).

They are complementary, not redundant. A designer reads `use_when` to decide whether to apply the pattern; they read `rationale` to understand and trust it.

---

## Array Notation for Repeating Layout Elements

Repeating structural units in `layout` use `[]` suffix — borrowed from the programming array convention:

```yaml
# [] = "one or more instances of this template"
body:
  - section[]:
      header:
        slots: [title, subtitle?, see_all_button]
      card_row:
        type: horizontal_scroll
        item: content_card[]
        content_card:
          slots: [hero_image, title, metadata_row, badge?]
```

Rules:
- `key[]` — a repeating structural unit; LLM generates N instances
- `key?` in a slots list — optional slot within a single component
- No `[]` — a singular, non-repeating component
- `count_range: [min, max]` — optional cardinality hint alongside `[]`

---

## Candidate Spec v3 — "Search Results Screen"

```yaml
---
# ─── Identity ────────────────────────────────────────────────────────
name: "Search Results Screen"
id: search/results
category: search        # onboard | configure | browse | search | discover |
                        # detail | create | transact | monitor

# ─── Context (selection conditions) ─────────────────────────────────
app_genres: [e-commerce, content, social, utility]
use_when: "user has submitted a query and expects a ranked list of matches"
not_when:
  - "results better shown on a map → Map Results Screen"
  - "visual appearance is the primary evaluator → Browse Grid Screen"

# ─── Layout (generative skeleton) ────────────────────────────────────
layout:
  header:
    - search_bar:
        state: shows current query; tappable to refine
        position: sticky top
    - filter_row:
        type: horizontal chip list
        behavior: scrollable; active filters highlighted
  body:
    - result_card[]:
        slots: [thumbnail?, title, subtitle, metadata_row, primary_action?]
  footer:
    - bottom_nav:
        type: bottom_nav        # bottom_nav | persistent_search | action_bar | combined
        optional: true

# ─── Interactions ─────────────────────────────────────────────────────
interactions:
  - "result_card tap → Detail Screen for that item"
  - "search_bar tap → activates keyboard; user can refine query"
  - "filter chip tap → filters result_card[] in place"
  - "bottom_nav tab tap → navigates to that top-level section"

# ─── Content ──────────────────────────────────────────────────────────
content:
  result_count_label: "e.g. '128 results for running shoes'"
  result_card_data: [name, price_or_date, rating_or_tag, image]
  empty_state: required — no-results message + suggested action

# ─── Constraints ──────────────────────────────────────────────────────
constraints:
  must:
    - "active query always visible in the search bar"
    - "result count or status shown below filter row"
    - "each card has exactly one primary action"
    - "empty state present when zero results"
  must_not:
    - "require horizontal scroll to read a result card"
    - "hide the filter row behind a non-obvious control"
  should:
    - "allow filter refinement without navigating away"
    - "support pull-to-refresh"
---

## Rationale
**Problem:** The user has stated an information need but cannot hold all candidate results in memory at once.  
**Forces:** Density vs. scannability — more results per view means less readable per item; filter access vs. content space — inline filters consume vertical room.  
**Resolution:** A sticky search bar keeps the active query in view; a horizontal chip row exposes filters without leaving the screen; result cards surface only enough information to judge relevance before committing to a detail view.

## Variants
- **Map Results**: swap `result_card[]` body for a map + bottom sheet list; use when results have location (restaurants, rentals, events)
- **Browse Grid**: swap for a 2–3 column image grid; use when visual appearance is the primary evaluator (fashion, art)
- **Mixed Feed**: interleave result types (products + articles + accounts); use for broad-discovery or social contexts

## Examples
- App Store search — list + icon, title, rating, price
- Amazon — thumbnail, title, price, Prime badge, rating
- Airbnb — map/list toggle
- Spotify — sectioned list of tracks, artists, playlists
```

---

## DoorDash Example — Updated with v3 Syntax

```yaml
---
name: "Curated Discovery Home Screen"
id: discover/curated-sections
category: discover

app_genres: [food-delivery, e-commerce, streaming, content]
use_when: "user opens the app with no specific intent and wants to see what's available or recommended"
not_when:
  - "user has a specific query → Search Results Screen"
  - "user selected a category → Browse List Screen"

layout:
  header:
    - category_tabs:
        type: horizontal_scroll_tabs
        behavior: pivots content sections below; active tab highlighted
  body:
    - section[]:
        count_range: [2, 6]
        header:
          slots: [title, subtitle?, see_all_button]
        card_row:
          type: horizontal_scroll
          item: content_card[]
          content_card:
            slots: [hero_image, title, metadata_row, badge?]
  footer:
    - search_bar:
        type: persistent_search       # not bottom_nav
        slots: [search_input, ai_action_button?]
        position: sticky bottom

interactions:
  - "category tab tap → pivots section[] content to that category"
  - "content_card tap → Detail Screen for that item"
  - "see_all_button tap → Browse Screen scoped to that section's theme"
  - "search_bar tap → Search Screen"

content:
  card_data: [name, rating, review_count, delivery_time, discount_badge?]
  empty_state: not applicable — show skeleton loader while content fetches

constraints:
  must:
    - "each section[] has a distinct curatorial theme"
    - "category tabs visually indicate the active selection"
  must_not:
    - "mix content_card[] types within a single section's horizontal row"
    - "require vertical scroll to reach the sticky search bar"
  should:
    - "load skeleton cards while content fetches"
    - "prioritize personalized or location-relevant content in first section"
---

## Rationale
**Problem:** Users open the app without a specific target and need to be shown what's available across many categories.  
**Forces:** Breadth vs. depth — more sections expose more themes but increase scroll distance; commitment cost — horizontal scroll lets users skim without reading deeply.  
**Resolution:** Vertically stacked curated sections let users compare themes at a glance; horizontal card rows within each section reduce per-section commitment while still surfacing enough detail (name, rating, time) to act.

## Variants
- **Single-section feed**: one continuous vertical scroll; use when app has one primary content type (TikTok, Twitter)
- **Grid sections**: 2-column grid per section instead of horizontal card row; use when visual variety is lower (music albums, podcast covers)

## Examples
- DoorDash home — food delivery, multiple curated sections
- Spotify home — "Jump back in," "Made for you," "New releases"
- App Store Today tab — curated editorial sections
```

---

## LLM Conditioning Interface (v3)

PatternMCP injects structured fields. The interaction block is now included.

```
[PATTERN: Search Results Screen | category: search]

Use when: user has submitted a query and expects a ranked list of matches

Layout:
  header: search_bar (sticky, shows query) + filter_row (horizontal chips)
  body:   result_card[] [thumbnail?, title, subtitle, metadata_row, action?]
  footer: bottom_nav (optional)

Interactions:
  - result_card tap → Detail Screen
  - filter chip tap → filters result_card[] in place
  - search_bar tap → activates keyboard for query refinement

Constraints (must):
  - active query always visible
  - result count shown
  - each card has one primary action
  - empty state required

Content: result_count_label, result_card (name, price/date, rating/tag, image), empty_state
```

Designer UI shows: `rationale`, `variants`, `examples` — for inspection and edit before generation.

---

## Minimum Viable Pattern Fields (v3)

```yaml
name: string
id: category/slug
category: enum (from mobile-ui-taxonomy intent dimension)
use_when: string
layout:
  header: ...
  body: ...   # use [] for repeating units
constraints:
  must: [...]
```

Optional but recommended: `not_when`, `app_genres`, `interactions`, `content`, `rationale`, `variants`, `examples`

---

## Open Questions (v3)

1. **`[]` cardinality:** Should `key[]` always allow 1..N, or should we distinguish `key[]` (one or more) from `key[]+` (two or more) or `key[2-6]` (bounded)? Simpler to just use an optional `count_range` alongside `[]`.

2. **`interactions` scope:** The current `interactions` block describes what happens on tap. Should it also capture swipe, long-press, and drag? Or restrict to tap-only for v1?

3. **`rationale` sub-labels:** The three labels (Problem / Forces / Resolution) map to Borchers. Should they be enforced as a template (designer must fill in all three), or kept as suggested structure within free prose?

4. **Navigation graph:** The journal note says "a pattern can contain knowledge about which other types of screens it can navigate to." The `interactions` block already encodes this (e.g., `card tap → Detail Screen`). Should there be an explicit `navigates_to: [id1, id2]` field for graph-level queries, separate from the prose interactions?

5. **Variant storage:** Inline (as above) or sibling files (`id: search/results-map`, `parent: search/results`)? Inline is simpler for authoring; sibling files are independently retrievable.
