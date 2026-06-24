# Pattern Specification Language — Revised Draft (v2)

> Addresses inline feedback on v1. Grounded in the literature survey + mobile-ui-taxonomy (https://github.com/ucla-hci/mobile-ui-taxonomy).

---

## Revisions from v1

| xac comment | Change in v2 |
|---|---|
| Mobile is scope, not goal | Framing adjusted; mobile used as initial domain for concreteness |
| Pattern must be generative | Added `layout` (component hierarchy) + `Sketch` section |
| Missing visual representation | `Sketch` section: ASCII diagram of prototypical screen |
| Name "Bottom Nav Bar" = element, not screen | New example: "Search Results Screen" — screen-level, intent-based |
| `trigger` semantics unclear | Renamed to `use_when` / `not_when` — conditions for selection |
| `related` purpose unclear | Dropped for MVP; can be added when catalog grows |
| Natural language body needs structure | `constraints` split into `must` / `must_not` / `should`; Forces as tension pairs |

---

## Pattern Unit: Intent × Screen

The **mobile-ui-taxonomy** organizes screens along two dimensions:

- **User intent**: onboard · configure · browse · search · discover · detail · create · transact · monitor
- **Product category**: social · e-commerce · streaming · finance · productivity · navigation

The unit of a pattern is one **user-intent screen type** — generic enough to apply across product categories, with `app_genres` as a context modifier.

This fixes the naming problem: patterns are named by intent, not by a component that happens to appear on the screen.

| ❌ Component-named (v1) | ✅ Intent-named (v2) |
|---|---|
| "Bottom Navigation Bar" | "Multi-Section Home Screen" |
| "Search Bar with List" | "Search Results Screen" |
| "Card Grid" | "Browse Grid Screen" |

---

## Section Map (Purpose of Each Field)

| Section | Purpose | Primary reader |
|---|---|---|
| `name`, `id`, `category` | Catalog indexing and retrieval | System |
| `use_when`, `not_when`, `app_genres` | Pattern selection — when to retrieve this pattern | System + Designer |
| `layout` | Component hierarchy for code generation — the generative skeleton | LLM |
| `constraints` (must / must_not / should) | Design rules LLM must respect | LLM |
| `content` | Content types that fill the components | LLM |
| Problem, Forces | Why the pattern exists; what tensions it resolves | Designer |
| Solution | Prose description of the pattern's resolution | Designer |
| Sketch | ASCII diagram of the prototypical screen | Designer |
| Variants | Alternative layouts for adjacent contexts | Designer + System |
| Examples | Real apps that instantiate this pattern | Designer |

---

## Candidate Spec v2 — "Search Results Screen"

```yaml
---
# ─── Identity ─────────────────────────────────────────────────────
name: "Search Results Screen"
id: search/results
category: search          # user intent: onboard | configure | browse | search |
                          #              discover | detail | create | transact | monitor

# ─── Context ──────────────────────────────────────────────────────
app_genres: [e-commerce, content, social, utility]
use_when: "user has submitted a query and expects a ranked list of matches"
not_when:
  - "results are better shown on a map → use Map Results Screen"
  - "visual appearance is the primary evaluator → use Browse Grid Screen"

# ─── Generative Structure ─────────────────────────────────────────
# This is the skeleton the LLM uses to generate UI code.
layout:
  header:
    - search_bar:
        state: shows current query; tappable to refine
        position: sticky top
    - filter_row:
        type: horizontal chip list
        behavior: scrollable; active filters highlighted
  body:
    - result_list:
        type: scrollable vertical list
        item: result_card
        result_card:
          slots: [thumbnail?, title, subtitle, metadata_row, primary_action?]
  footer:
    - bottom_nav: optional; include if app has 3+ top-level sections

# ─── Content ──────────────────────────────────────────────────────
# What fills the components — helps LLM generate realistic placeholder content.
content:
  result_count_label: "e.g. '128 results for running shoes'"
  result_card_data: [name, price_or_date, rating_or_tag, image]
  empty_state: required — no-results message + suggested action (refine query / browse)

# ─── Constraints ──────────────────────────────────────────────────
constraints:
  must:
    - "active query is always visible in the search bar"
    - "result count or status shown below filter row"
    - "each card has exactly one clear primary action (tap → Detail Screen)"
    - "empty state present when zero results"
  must_not:
    - "require horizontal scroll to read a result card"
    - "hide the filter row behind a non-obvious control"
  should:
    - "allow filter refinement without navigating away from results"
    - "support pull-to-refresh"
---

[xac: which section (#) do the following belong to? are they trying to inform a designer when to use this pattern? almost like some metadata?]

## Problem
The user has stated an information need and must evaluate multiple candidates to find the best match — but cannot hold all options in memory at once.

## Forces
- Density vs. scannability: more results per screen → less readable per item
- Filter access vs. content space: inline filters take vertical space from results
- Speed vs. richness: users want fast scanning; enough detail to decide without opening each item

## Solution
A persistent search bar at the top keeps the active query visible and re-enterable. A horizontal chip row exposes the most common filter axes without leaving the screen. The body is a scrollable list of result cards, each surfacing just enough information (title + key metadata) to judge relevance. Tapping a card opens the Detail Screen.

## Sketch
[xac: not necessary as it's hard to fully depict a pattern by hacking text]

```
┌──────────────────────────────┐
│ 🔍  running shoes         ✕  │  ← search_bar (sticky, editable)
├──────────────────────────────┤
│ [All] [Men's] [<$100] [★4+]  │  ← filter_row (horizontal scroll)
├──────────────────────────────┤
│  128 results                 │  ← result_count_label
│  ┌──────────────────────┐    │
│  │ 🖼  Nike Air Zoom    │    │  ← result_card
│  │     $89  ★4.5  Free  │    │
│  └──────────────────────┘    │
│  ┌──────────────────────┐    │
│  │ 🖼  Adidas Ultraboost │    │
│  │     $110  ★4.2       │    │
│  └──────────────────────┘    │
│  ···                         │
├──────────────────────────────┤
│  🏠    🔍    ❤️    👤        │  ← bottom_nav (if applicable)
└──────────────────────────────┘
```

## Variants
- **Map Results**: replace `result_list` body with a map + bottom sheet of results; use when results have a location (restaurants, rentals, events)
- **Browse Grid**: replace `result_list` with 2–3 column image grid; use when visual appearance is the primary evaluation criterion (fashion, art)
- **Mixed Feed**: interleave result types (products + articles + accounts); use for social or broad-discovery contexts

## Examples
- App Store search (list + icon, title, rating, price)
- Amazon (list + thumbnail, title, price, Prime badge, rating)
- Airbnb (map + list toggle)
- Spotify (list of tracks + artists + playlists, sectioned)
```

---

## LLM Conditioning Interface

PatternMCP injects the structured fields into the prompt. The body prose is surfaced in the designer UI.

**Injected block (system context):**
```
[PATTERN: Search Results Screen | category: search]

Use when: user has submitted a query and expects a ranked list of matches

Layout:
  header: search_bar (sticky, shows query) + filter_row (horizontal chips)
  body:   result_list → result_card [thumbnail?, title, subtitle, metadata_row, action?]
  footer: bottom_nav (optional)

Constraints (must):
  - active query always visible in search bar
  - result count shown
  - each card has one primary action (→ Detail Screen)
  - empty state required

Content: result_count_label, result_card (name, price/date, rating/tag, image), empty_state
```

**Designer UI exposes:** Problem, Forces, Solution, Sketch — for inspection and edit before generation.

---

## Minimum Viable Pattern Fields

For initial catalog authoring, the required fields are:

```yaml
name: string
id: category/slug
category: enum (from mobile-ui-taxonomy intent dimension)
use_when: string
layout:           # generative skeleton — required
  header: ...
  body: ...
constraints:
  must: [...]
```

Everything else (not_when, app_genres, content, Sketch, Forces, Variants, Examples) is recommended but optional.

---

## Open Questions (Revised)

1. **Layout field fidelity:** The `layout` YAML above uses natural-language slot descriptions. Should it be stricter — e.g., a formal component tree with typed nodes? Tradeoff: stricter = more generative power, harder to author.

2. **Sketch format:** ASCII diagram works for text-only specs. For the PatternGenUI front-end, would a Figma frame or a component-tree SVG be worth generating alongside? Or is ASCII enough for v1?

3. **Category taxonomy:** Use mobile-ui-taxonomy's 10 intents (onboard/configure/browse/search/discover/detail/create/transact/monitor) as the canonical `category` enum? Or extend/modify?

4. **Variant handling:** Variants share most fields with the parent. Store inline (as above) or as sibling files (`id: search/results-map`, parent: `search/results`)? Inline is simpler; siblings are more retrievable independently.

5. **Generativity check:** Does the `layout` field as written give an LLM enough to produce a plausible React Native / SwiftUI component tree? Should we validate this empirically with a quick generation test before committing to the schema?
