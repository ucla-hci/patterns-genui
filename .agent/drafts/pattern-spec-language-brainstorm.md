# Brainstorm: Pattern Specification Language for PatternGenUI

> Grounded in survey of 14 papers (Borchers 2000 → Nguyen 2018). See `literature/_approaches-design-pattern-catalogs.md`.

---

## What the Literature Tells Us We Need

Every prior spec is deficient in at least one axis:

| Prior work | What it gets right | What it misses |
|---|---|---|
| Borchers template | Name/context/problem/solution/forces | Not machine-readable; not mobile [xac: note that mobile is not our goal; it's for scoping the focus; also mobile ui is probably the most frequently used ui (need stats verification)] |
| Sinnig XPLML | Machine-readable XML | Widget-level only; not designer-readable |
| Neil catalog | Screen-level; mobile; visual | Prose only; no retrieval structure |
| Tidwell | Designer-readable; named | No schema; not mobile-first |

The target: **dual-readable** — a designer can read it, an LLM can be conditioned by it, and a retrieval system can index it.
[xac: in addition to machine-readable, the pattern should be generative, meaning it lends itself naturally to generating ui code of a screen of a given pattern]
[xac: another important consideration is that the pattern should concisely abstract all the important information such as the ones proposed by Brochers]

[xac: here's a curation of mobile screens for your inspiration: https://github.com/ucla-hci/mobile-ui-taxonomy]

---

## Core Design Decisions

### 1. Unit of a Pattern

The journal says "each pattern can focus on a mobile screen." Three interpretations:

| Option | Example | Notes |
|---|---|---|
| **Screen type** | "Search Results Screen" | Atomic; aligns with Neil (2014) |
| Interaction concern | "Search & Filter" | May span multiple screens |
| Screen + trigger | "Screen shown after user submits a query" | Most precise; verbose |

**Recommendation:** screen type, with a `trigger` field capturing the user action that surfaces it.

### 2. Format

| Format | Designer-readable | Machine-parseable | Authoring effort |
|---|---|---|---|
| Pure JSON/YAML | Low | High | Low |
| Custom DSL | Medium | Medium | High |
| **YAML frontmatter + Markdown body** | **High** | **High** | **Low** |

**Recommendation:** YAML frontmatter + Markdown body. No new tooling; frontmatter is machine-readable; body is designer prose.

### 3. Granularity Hierarchy

Three levels (borrowing van Welie's hierarchy):

```
Category  (e.g., Navigation)
  └── Pattern  (e.g., Bottom Navigation Bar)
        └── Variant  (e.g., Bottom Nav with Badge)
```

Each *pattern file* is one screen-level construct. Variants are sub-entries within the same file.

---

## Candidate Spec (Draft)

[xac: overall, 1) unclear what's the purposes of some sections; 2) seems to be missing a prototypical representation of the pattern (hard to visualize what the screen might be like when reading the pattern)]


```yaml
---
# ── Identity ──────────────────────────────────────────────────────
name: "Bottom Navigation Bar" [xac: the name sounds like describing an element of a screen rather than a whole screen]
aliases: ["Tab Bar", "Bottom Tabs"]
id: nav/bottom-navigation-bar        # category/slug for stable references

# ── Classification ────────────────────────────────────────────────
category: navigation                 # Neil's 11: navigation | forms | search |
                                     # sort-filter | tools-actions | charts |
                                     # invitations | feedback | help | anti-pattern
screen_type: persistent-chrome       # how it sits on the screen
app_genres: [social, e-commerce, content, utility]

# ── Trigger & Context ─────────────────────────────────────────────
trigger: "app has 3–5 top-level destinations" [xac: how would this be interpreted when instantiating this pattern?]
preconditions:
  - "destinations are peer-level (not hierarchical)"
  - "user navigates frequently between sections"

# ── Structure ─────────────────────────────────────────────────────
elements:
  required:
    - icon-label pairs: "3–5 items, always visible"
    - active indicator: "highlights current destination"
  optional:
    - badge: "numeric or dot, for notifications"
    - floating action button: "if primary action is global"

constraints:
  - "thumb-reachable at screen bottom"
  - "icons must be recognizable without labels (but include labels)"
  - "no more than 5 items — use hamburger if more needed"
  - "persists across all screens within each section"

# ── Relations ─────────────────────────────────────────────────────
[xac: not sure what purpose is this]
related:
  enables: ["Top App Bar", "Section Home Screen"]
  conflicts: ["Hamburger Menu"]
  often_paired: ["FAB", "Badge"]

# ── Retrieval ─────────────────────────────────────────────────────
tags: [navigation, tabs, persistent, mobile, iOS, Android]
keywords: "tab bar bottom navigation persistent global"
---

[xac: some of the info below is purely natural language. should there be a more specific format for filling in those info?]

## Problem
The app has multiple top-level sections and users need to move between them 
frequently without losing their place within each section.

## Forces
- Discoverability vs. clutter: more tabs = more visible options, but more cognitive load
- Reach vs. space: bottom placement is thumb-friendly but consumes vertical space
- Consistency vs. context: the bar must stay constant even when section content scrolls

## Solution
Place 3–5 labeled icon buttons in a fixed bar along the bottom edge of the screen. 
The active tab is visually distinguished. Tapping a tab navigates to that section's 
root; tapping again scrolls to top or resets state.

## Use When
- App has 3–5 peer-level top-level destinations
- Users visit multiple destinations per session
- **Not when**: destinations are sequential (use Stepper) or hierarchical (use Sidebar)

## Consequences
- (+) Immediate visibility of top-level structure; low navigation cost
- (+) Thumb-reachable on all phone sizes
- (-) Consumes ~50dp of vertical space on every screen
- (-) Degrades above 5 items; awkward with 2 items

## Variants
- **Badge variant**: adds a red dot or count badge to a tab for notifications
- **FAB overlay**: a floating action button centered above the bar for a primary action

## Examples
- Instagram (Home, Search, Reels, Shop, Profile)
- Google Maps (Explore, Commute, Saved, Contribute)
```

---

## LLM Conditioning Interface

When PatternMCP retrieves this pattern, it injects a structured block into the LLM prompt:

```
[DESIGN PATTERN: Bottom Navigation Bar]
Category: navigation | Screen type: persistent-chrome
Trigger: app has 3–5 top-level destinations
Required elements: icon-label pairs (3–5), active indicator
Constraints:
  - thumb-reachable at screen bottom
  - no more than 5 items
  - persists across all screens within each section
Often paired with: FAB, Badge
```

The prose body (Problem, Forces, Solution) is optionally appended for richer conditioning — or surfaced in the designer UI for inspection and edit.

---

## Open Decisions

1. **Minimum viable fields?** The spec above has 10+ frontmatter fields. A leaner MVP: `name`, `category`, `trigger`, `elements.required`, `constraints` only.

2. **Variant storage:** Inline (as above) vs. separate files with `parent: nav/bottom-navigation-bar`. Separate is cleaner for retrieval but multiplies catalog size.

3. **Designer-editable fields:** Which frontmatter fields should be editable in the PatternGenUI UI? `constraints` and `elements` are the most useful levers.

4. **Visual layout hints:** E.g., `layout: fixed-bottom | overlay | inline` — useful for LLM generation but adds another maintenance axis.

5. **ID scheme:** `category/slug` (human-readable, brittle to renames) vs. UUID (stable, opaque). Recommend `category/slug` for authoring + a separate stable `uid` for cross-references.
