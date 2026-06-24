# Pattern Specification Language — v4 (Current)

> Final form after four iterations and two application tests (DoorDash, Chase).
> This is the working spec. Prior drafts (v1–v3) are kept for reference.

---

## Design Principles

Six principles codified through the iteration process:

1. **Intent × topology, not domain template.** A pattern is a layout topology that satisfies a user intent. The same pattern applies across product domains; domain-specific fill belongs in `content` and `examples`, not `layout`.

2. **`layout` names are domain-neutral.** Use abstract component names (`item_group[]`, `content_card[]`, `result_card[]`). Never use domain terms (`account_row`, `product_tile`) in `layout`.

3. **`[]` suffix marks repeating structures.** `key[]` means "one or more instances of this structural template." Optional cardinality hint: `count_range: [min, max]` alongside `[]`.

4. **`use_when` ≠ `rationale`.** `use_when`/`not_when` are machine-readable selection conditions (when to retrieve this pattern). `rationale` is designer-facing design reasoning (why this layout resolves the tension). They answer different questions: *when* vs. *why*.

5. **`role: action | filter` on header chip rows.** Horizontally-scrolling chip rows appear in both filter and action roles; the role is semantically critical for LLM code generation and must be explicit.

6. **`interactions` is typed, not freeform.** Each entry specifies `component`, `on` (event), `action` (navigate or effect), and a `target` or `effect` description. This enables navigation graph queries and correct event handler generation.

---

## Section Map

| Section | Purpose | Reader | In LLM prompt? |
|---|---|---|---|
| `name`, `id`, `category` | Catalog indexing and retrieval | System | Yes |
| `use_when`, `not_when`, `app_genres` | Selection conditions — when to retrieve this pattern | System + Designer | Yes |
| `layout` | Generative skeleton — abstract component hierarchy for code generation | LLM | Yes |
| `interactions` | Typed interaction flows — what happens when the user acts | LLM | Yes |
| `content` | Domain-specific fill examples for the abstract layout slots | LLM | Yes |
| `constraints` (must / must_not / should) | Design rules the generated UI must respect | LLM | Yes |
| `rationale` | Design reasoning — what tensions this pattern resolves and how | Designer | No (UI only) |
| `variants` | Adjacent layout alternatives with different topologies | Designer + System | No |
| `examples` | Real products that instantiate this pattern | Designer | No |

---

## Format Reference

### Array notation

```yaml
# [] = one or more instances of this structural template
body:
  - item_group[]:
      count_range: [2, 6]   # optional cardinality hint
      group_header:
        slots: [group_name, item_count?, aggregate_value?]
      item_row[]:
        slots: [item_name, item_id?, primary_value, primary_value_label, trend_indicator?, status_badge?]
```

- `key[]` — repeating unit; LLM generates N instances
- `key?` in a slots list — optional slot within a single component
- No suffix — singular, non-repeating component

### Header chip role

```yaml
header:
  - action_chip[]:
      type: horizontal_scroll
      role: action    # action: triggers an operation | filter: pivots/filters body content
      slots: [icon?, label]
```

### Interactions schema

```yaml
interactions:
  - component: item_row        # component name from layout
    on: tap                    # tap | swipe | long_press
    action: navigate           # navigate | effect
    target_label: Detail Screen
    target_id: detail/item     # optional — pattern catalog ID for navigation graph
  - component: group_header
    on: tap
    action: effect
    effect: toggles group expand/collapse
```

### Rationale structure

```markdown
## Rationale
**Problem:** [one sentence — the user need this layout addresses]
**Forces:** [the tensions that constrain the solution]
**Resolution:** [how the layout resolves those tensions]
```

### Footer type enum

```yaml
footer:
  - nav_bar:
      type: bottom_nav | persistent_search | action_bar | combined
```

---

## Full Spec — "Search Results Screen"

```yaml
---
# ─── Identity ────────────────────────────────────────────────────────
name: "Search Results Screen"
id: search/results
category: search        # onboard | configure | browse | search | discover |
                        # detail | create | transact | monitor

# ─── Context ─────────────────────────────────────────────────────────
app_genres: [e-commerce, content, social, utility]
use_when: "user has submitted a query and expects a ranked list of matches"
not_when:
  - "results better shown on a map → Map Results Screen"
  - "visual appearance is the primary evaluator → Browse Grid Screen"

# ─── Layout ──────────────────────────────────────────────────────────
layout:
  header:
    - search_bar:
        state: shows current query; tappable to refine
        position: sticky top
    - filter_chip[]:
        type: horizontal_scroll
        role: filter
        slots: [label, active_indicator]
  body:
    - result_card[]:
        slots: [thumbnail?, title, subtitle, metadata_row, primary_action?]
  footer:
    - nav_bar:
        type: bottom_nav
        optional: true

# ─── Interactions ────────────────────────────────────────────────────
interactions:
  - component: result_card
    on: tap
    action: navigate
    target_label: Detail Screen
    target_id: detail/item
  - component: search_bar
    on: tap
    action: effect
    effect: activates keyboard; user can refine query
  - component: filter_chip
    on: tap
    action: effect
    effect: filters result_card[] in place
  - component: nav_bar
    on: tap
    action: navigate
    target_label: "{tab.section} Screen"

# ─── Content ─────────────────────────────────────────────────────────
content:
  # Fill examples — domain-neutral layout, domain-specific data
  result_card_data: [name, price_or_date, rating_or_tag, image]
  result_count_label: "e.g. '128 results for running shoes'"
  empty_state: required — no-results message + suggested action (refine / browse)

# ─── Constraints ─────────────────────────────────────────────────────
constraints:
  must:
    - "active query always visible in the search_bar"
    - "result count or status shown near filter_chip[] row"
    - "each result_card has exactly one primary action"
    - "empty state present when zero results"
  must_not:
    - "require horizontal scroll to read a result_card"
    - "hide filter_chip[] behind a non-obvious control"
  should:
    - "allow filter refinement without navigating away"
    - "support pull-to-refresh"
---

## Rationale
**Problem:** The user has stated an information need but cannot hold all candidate results in memory at once.
**Forces:** Density vs. scannability — more results per view means less readable per item; filter access vs. content space — inline filters consume vertical room.
**Resolution:** A sticky search bar keeps the active query in view; a horizontal filter chip row exposes common axes without leaving the screen; result cards surface only enough information (title + key metadata) to judge relevance before tapping through.

## Variants
- **Map Results**: swap `result_card[]` body for a map + bottom-sheet list; use when results have location (restaurants, rentals, events)
- **Browse Grid**: swap for a 2–3 column image grid; use when visual appearance is the primary evaluator (fashion, art)
- **Mixed Feed**: interleave result types (products + articles + accounts); use for broad-discovery or social contexts

## Examples
- App Store search — list + icon, title, rating, price
- Amazon — thumbnail, title, price, Prime badge, rating
- Airbnb — map/list toggle
- Spotify — sectioned list of tracks, artists, playlists
```

---

## LLM Conditioning Block

PatternMCP injects the following into the system prompt. `rationale`, `variants`, and `examples` are surfaced in the designer UI only.

```
[PATTERN: Search Results Screen | id: search/results | category: search]

Use when: user has submitted a query and expects a ranked list of matches

Layout:
  header: search_bar (sticky, shows query) + filter_chip[] (role: filter, horizontal scroll)
  body:   result_card[] [thumbnail?, title, subtitle, metadata_row, primary_action?]
  footer: nav_bar (bottom_nav, optional)

Interactions:
  result_card tap → navigate: Detail Screen
  search_bar tap → effect: activates keyboard for query refinement
  filter_chip tap → effect: filters result_card[] in place

Constraints (must):
  - active query always visible
  - result count shown
  - each card has one primary action
  - empty state required

Content: result_card (name, price/date, rating/tag, image), result_count_label, empty_state
```

---

## Minimum Viable Pattern Fields

```yaml
name: string
id: category/slug
category: enum  # mobile-ui-taxonomy intent dimension
use_when: string
layout:
  header: ...   # at least one component
  body: ...     # at least one component; use [] for repeating units
constraints:
  must: [...]
```

Optional (recommended): `not_when`, `app_genres`, `interactions`, `content`, `rationale`, `variants`, `examples`

---

## Open Questions

| Question | Status | Notes |
|---|---|---|
| `target_id` typed references | Open | Enables navigation graph; requires catalog to exist first |
| `role` required vs optional on chip rows | Open | Required when both filter and action chip rows coexist on one screen |
| `key[]` vs `key[min-max]` cardinality | Resolved | Use `key[]` + optional `count_range: [min, max]` alongside |
| Sketch / ASCII diagram | Resolved — dropped | Hard to author; not worth the cost |
| Problem/Forces/Solution as three sections | Resolved — collapsed | Now `rationale` with labeled prose sub-parts |
| Variant storage: inline vs sibling files | Open | Inline for v1; sibling files when catalog grows and variants need independent retrieval |
| `status_badge` in `layout` vs `content` | Open | Currently in `content`; may move to `layout` if presence/absence affects structure |
