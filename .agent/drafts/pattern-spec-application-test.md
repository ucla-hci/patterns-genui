# Pattern Spec — Application Test

> Applying the v2 spec language to the attached DoorDash screenshot.
> Goal: find what holds, what breaks, what needs to change.

---

## Screen Analysis

The screen is a **food delivery home screen** (DoorDash). Key components top to bottom:

1. **Category tab row** — horizontal scroll: Grocery · Deals · Member Deals · Going Out · Reservations
2. **Section 1: "Explore Local Favorites"** — section header (title + subtitle + see-all arrow) + horizontal card row (restaurant cards: video/photo, name, heart, rating, count, time, discount badge)
3. **Section 2: "Summer savings with DashPass"** — same section structure, different card content (promotional lifestyle images, wider cards)
4. **Sticky bottom bar** — search input ("Search Water") + "Ask" AI button

User intent from the taxonomy: **discover** (user opened the app with no specific target; wants to be shown what's available).

---

## Applied Spec

```yaml
---
name: "Curated Discovery Home Screen"
id: discover/curated-sections
category: discover

app_genres: [food-delivery, e-commerce, streaming, content]
[xac: is use_when and not_when related to rationale?]
use_when: "user opens the app with no specific intent and wants to see what's available or recommended"
not_when:
  - "user has a specific item/query → Search Results Screen"
  - "user selected a category → Browse List or Browse Grid Screen"

layout:
  header:
    - category_tabs:
        type: horizontal_scroll_tabs
        behavior: filters or pivots content sections below; active tab highlighted
  body:
    - content_sections:            # ← THIS IS THE PROBLEM (see notes below)
        type: vertical_stack
        item: section
        section: [xac: there could be a format to specify repeated elements, similar to how an array in programming language works]
          header:
            slots: [title, subtitle?, see_all_button]
          card_row:
            type: horizontal_scroll
            item: content_card
            content_card:
              slots: [hero_image, title, metadata_row, badge?]

  footer:
    - persistent_search_bar:       # ← footer is not bottom_nav here
        slots: [search_input, ai_action_button?]
        position: sticky bottom

content:
  card_data: [name, rating, review_count, delivery_time, discount_badge?]
  section_count: 2-6
  empty_state: not applicable (discovery screens always have content; show skeleton loader)

constraints:
  must:
    - "each section has a distinct curatorial theme (e.g., 'Local Favorites', 'Promotions')"
    - "each card is tappable → navigates to Detail Screen" [xac: here's an open problem--how can the pattern describe such interactive behavior? should it simply instruct llm to create an (empty) tap event handler]
    - "category tabs visually indicate the active selection"
    - "section 'see all' navigates to a full Browse Screen for that category"
  must_not:
    - "mix card types within a single section's horizontal row"
    - "require vertical scroll to reach the search bar (it is sticky)"
  should:
    - "load skeleton cards while content fetches"
    - "prioritize personalized or location-relevant content in first section"
---

## Rationale
Users arriving at a discovery screen haven't committed to a specific need yet.
The curated-sections layout lets them skim many options across multiple themes
quickly — horizontal scroll reduces commitment cost per section while vertical
scroll lets them compare themes.
[xac: is it worth breaking this down into specific attributes based on Borchers/Tidwell's work?]

## Variants
- **Single-section feed**: one continuous vertical scroll instead of multiple horizontal sections; use for apps with one primary content type (e.g., TikTok, Twitter)
- **Grid sections**: replace horizontal card rows with 2-column grid sections; use when visual variety is lower (e.g., music albums, podcast covers)

## Examples
- DoorDash home (food delivery, multiple curated sections)
- Spotify home (music discovery, sections: "Jump back in", "Made for you", etc.)
- App Store Today tab (curated editorial sections)
```

---

## What Works ✓

| Field | Status |
|---|---|
| `name`, `id`, `category` | ✓ Clean fit |
| `use_when` / `not_when` | ✓ Easy to fill; immediately actionable |
| `app_genres` | ✓ Works |
| `constraints` (must/must_not/should) | ✓ Natural to express; directly useful for LLM |
| `content.card_data` | ✓ Works; slots match actual screen |
| `Rationale` (collapsed from Problem/Forces/Solution) | ✓ Much cleaner than three separate sections |

---

## What Breaks ✗

### 1. `layout.body` can't express repeating sections

The body here is **N instances of the same section template**, not a single component type. The v2 spec has no way to say "repeat this structure."

Current workaround above: `content_sections → item: section → section: {header, card_row}`. But this is awkward — the `layout` field mixes structural description with cardinality.

**Proposed fix:** Add an explicit `repeat` or `template` concept:
```yaml
body:
  - curated_sections:
      repeat: true          # marks this as a repeating structural unit
      count_range: [2, 6]
      unit:
        header: {slots: [title, subtitle?, see_all_button]}
        card_row: {type: horizontal_scroll, item: content_card}
```

---

### 2. `layout.footer` assumed to be bottom_nav

The footer in the v2 spec defaulted to `bottom_nav`. Here it's a **floating search bar + AI action**. The footer role changes per pattern.

**Proposed fix:** Generalize `footer.type`:
```yaml
footer:
  type: bottom_nav | persistent_search | action_bar | combined
```

And allow the footer to describe its slots freely, same as header.

---

### 3. Cards within sections have different schemas

Section 1 cards: `[video_thumbnail, name, heart_button, rating, time, discount_badge]`
Section 2 cards: `[lifestyle_image, name, rating, distance, price_badge]` — wider, no time field

The current `content_card.slots` is one schema shared across all sections. In reality, **different sections can have structurally different cards**.

**Proposed fix:** Allow `section` to override `content_card.slots`:
```yaml
sections:
  default_card_slots: [hero_image, title, metadata_row, badge?]
  section_overrides:
    - theme: promotional
      card_slots: [hero_image_wide, title, badge_prominent]
```
Or: accept that within-section card consistency is a constraint (cards within one section must share a schema), which is already captured in `must_not: "mix card types within a single section."` Heterogeneity across sections is expected and doesn't need to be fully specified.

**Simpler fix:** Keep `content_card.slots` as the default, note that sections may override, and let the LLM handle the variation given the `content.card_data` field and the curatorial theme.

---

### 4. Interaction behaviors are missing

This screen has meaningful interactions not captured in `constraints`:
- Tapping a category tab pivots the content sections
- Each section is independently horizontally scrollable
- "See all" navigates to a Browse Screen

`constraints` captures **design rules** (what must be true at rest) but not **interaction flows** (what happens when the user acts).

**Proposed fix:** Add an optional `interactions` block:
```yaml
interactions:
  - "category tab tap → filters/pivots body content sections"
  - "card tap → Detail Screen for that item"
  - "see_all tap → Browse Screen scoped to section theme"
  - "search bar tap → Search Screen"
```
This is also useful for LLM generation (the LLM needs to know what each component does, not just what it looks like).

---

### 5. Problem / Forces / Solution → collapse into `rationale`

The user's comment confirms these three sections feel like metadata whose purpose is unclear. They are designer-facing documentation, not generative signals.

**Proposed fix:** Replace with a single `rationale` field (1–3 sentences max), shown in the designer UI but not injected into the LLM prompt.

---

## Summary of Proposed v3 Changes

| Change | From | To |
|---|---|---|
| Repeating body structures | No support | `repeat: true` + `count_range` on layout units |
| Footer type | hardcoded `bottom_nav` | `type: bottom_nav \| persistent_search \| action_bar` |
| Problem / Forces / Solution | Three separate prose sections | Single `rationale` field (1–3 sentences) |
| Sketch | ASCII diagram | Dropped |
| Interaction behaviors | Missing | Optional `interactions` list |
| Card schema variation | Single shared schema | Default + optional section overrides |
