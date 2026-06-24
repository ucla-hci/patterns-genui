# Pattern Spec — Application Test 2 (Chase)

> Applying v3 spec language to the Chase bank account overview screen.
> Revised after feedback: generalize component names; structure interactions.

---

## Screen Analysis

Chase mobile banking home screen. Components top to bottom:

1. **App bar** — Chase logo (center), chat icon + add-account icon (left), profile icon (right)
2. **Quick action chips** — horizontal scroll row: `+` · Deposit checks · Positions · See bill activity
3. **"Accounts" section header** — bold label + overflow menu (`...`)
4. **Account group: "Bank accounts (2)"** — dark blue header with group name + aggregate ($8,780.69), then:
   - TOTAL CHECKING (...0731) — $2,030.68 available balance + sparkline
   - TOTAL CHECKING (...8006) — $6,750.01 available balance + sparkline
5. **Account group: "Credit cards (4)"** — dark blue header + aggregate ($11,382.95), then:
   - United Gateway (...4285) — card image + $0.00 current balance + "You've set up automatic payments"
   - Prime Visa (...2770) — partially visible, cut off at bottom
6. **Bottom nav** — Accounts (active) · Pay & transfer · Plan & track · Benefits & travel · More

User intent: **monitor** — user has logged in to check the current state of their items.

---

## Applied Spec

```yaml
---
name: "Grouped Item Monitor Screen"
id: monitor/grouped-items
category: monitor

app_genres: [finance, health, productivity, portfolio]
use_when: "user wants to check the current state of a set of items they own or track, organized by category"
not_when:
  - "user wants history or detail for one item → Detail Screen"
  - "user wants to act on an item → Transact Screen"
  - "items belong to one category only → Single-Category Monitor Screen"

layout:
  header:
    - app_bar:
        slots: [utility_icon[]?, brand_logo, profile_icon]
        position: sticky top
    - quick_action_chip[]:
        type: horizontal_scroll
        role: action              # action | filter — NOT a content filter (contrast: DoorDash category tabs)
        slots: [icon?, label]
  body:
    - item_group[]:
        group_header:
          slots: [group_name, item_count?, aggregate_value?]
          style: prominent (visually separates groups; often colored)
        item_row[]:
          slots: [item_name, item_id?, primary_value, primary_value_label, trend_indicator?, status_badge?]
  footer:
    - bottom_nav:
        type: bottom_nav
        active_indicator: required

interactions:
  - component: item_row
    on: tap
    action: navigate
    target: Detail Screen
  - component: quick_action_chip
    on: tap
    action: navigate
    target: "{chip.label} Screen"
  - component: group_header
    on: tap
    action: effect
    effect: toggles group expand/collapse
  - component: bottom_nav_tab
    on: tap
    action: navigate
    target: "{tab.section} Screen"

content:
  # Abstract slots — filled differently per product (see cross-product table below)
  item_row_data: [item_name, item_id?, primary_value, primary_value_label]
  group_data: [group_name, item_count?, aggregate_value?]
  status_badge: optional — state-dependent inline message; type: success | warning | alert
  empty_state: not applicable — user always has tracked items after authentication

constraints:
  must:
    - "each item_row shows primary_value without a tap"
    - "group_header visually separates groups; aggregate_value shown if meaningful"
    - "active bottom_nav tab is highlighted"
  must_not:
    - "use horizontal scroll for item_row[] — data density requires full-width rows"
    - "hide primary_value behind a tap or toggle"
  should:
    - "show trend_indicator per item_row for temporal context at a glance"
    - "surface status_badge inline on the relevant item_row"
    - "support pull-to-refresh for live data"
---

## Rationale
**Problem:** The user needs to check the current state of multiple items across categories without opening each one individually.  
**Forces:** Completeness vs. scannability — showing all items at once risks overload; grouping by category lets the eye jump to the right section. Monitoring vs. acting — primary intent is read-only, but common operations should be reachable without mode-switching.  
**Resolution:** Prominent group headers with aggregate values let the user spot category-level issues instantly; item rows surface only the primary value needed to decide whether to drill in; quick action chips keep frequent operations accessible without cluttering the monitor view.

## Variants
- **Collapsed groups**: groups start collapsed (header only); user taps to expand; use when item count is large (5+ groups)
- **Aggregate-first**: a top-level summary row above all groups showing total across all items; use when the overall total is the primary signal (portfolio net worth, total calories)
- **Alert-first**: surfaces items with active alerts at the top of their group; use when status monitoring outweighs balance scanning

## Examples
- Chase — Bank accounts + credit cards; aggregate per group
- Robinhood — Stocks + ETFs + Crypto; portfolio value per group
- Apple Health — Activity + Mindfulness + Nutrition; daily totals per group
- Asana — Projects grouped by team; task counts per group
```

---

## Cross-Product Applicability

The same pattern — `monitor/grouped-items` — maps to all four products. Only the `content` slot fillers differ:

| Domain | `item_group[]` label | `item_row[]` | `primary_value` | `aggregate_value` | `quick_action_chip[]` |
|---|---|---|---|---|---|
| Chase (banking) | "Bank accounts", "Credit cards" | Account row | Balance | Total balance | Deposit, Transfer |
| Robinhood (portfolio) | "Stocks", "ETFs", "Crypto" | Holding row | Price / % change | Group value | Buy, Deposit |
| Apple Health | "Activity", "Mindfulness" | Metric row | Steps / ring % | Daily total | Add data |
| Asana (tasks) | "Design", "Engineering" | Task row | Status / due date | Open task count | + New task |
| Apple Home (IoT) | "Living room", "Bedroom" | Device row | On/off / temperature | Devices active | Scenes |

All five share the same layout topology: prominent group headers → vertical item rows → quick action chips → standard bottom nav. The pattern is the topology; the domain is the fill.

This confirms the design principle: **`layout` names abstract components, `content` holds domain-specific fill examples, `examples` shows which products instantiate the pattern.**

---

## Differentiation from DoorDash

| Dimension | DoorDash — `discover/curated-sections` | Chase — `monitor/grouped-items` |
|---|---|---|
| User intent | discover | monitor |
| Content source | Platform-curated editorial | User's own tracked data |
| Body nesting | `section[]` → horizontal `content_card[]` (2D) | `item_group[]` → vertical `item_row[]` (1D nested) |
| Scroll axis in body | Horizontal within each section | Vertical only |
| Header chips `role` | `filter` — pivots body content | `action` — triggers an operation |
| Group header slots | `[title, subtitle?, see_all_button]` | `[group_name, item_count?, aggregate_value?]` |
| Row content | Rich media (image, rating, time) | Data-dense (value, id, trend, status) |
| Footer | `persistent_search` | `bottom_nav` |

The `role: action | filter` field on `quick_action_chip[]` now makes the semantic difference machine-readable, not just prose-annotated.

---

## What Works in v3 ✓

| Field | Status |
|---|---|
| `[]` array notation | ✓ `item_group[]` and `item_row[]` are abstract and composable |
| `footer.type: bottom_nav` | ✓ Clean fit |
| `interactions` structured schema | ✓ `{component, on, action, target/effect}` — machine-parseable, LLM-friendly |
| `rationale` (Problem/Forces/Resolution) | ✓ Three tensions present and distinct |
| `use_when` / `not_when` | ✓ Intent-level conditions, product-agnostic |

---

## Remaining Stresses

### 1. `role: action | filter` on header chips — should this be a spec-level field?

The `role` attribute on `quick_action_chip[]` was added in this revision to make the semantic difference from DoorDash's category tabs machine-readable. It should be codified in v4 as a required field on any horizontally-scrolling chip row in the header, since both `filter` and `action` roles are common and the LLM must generate different event handlers for each.

### 2. `interactions.target` references are strings, not pattern IDs

`target: "Detail Screen"` is a human-readable label, not a pattern `id` like `detail/account`. For the navigation graph use case (journal: "a pattern can contain knowledge about which other types of screens it can navigate to"), targets should eventually be typed as pattern IDs so the system can answer "what screens does this pattern link to?" programmatically.

Proposal for a later revision:
```yaml
- component: item_row
  on: tap
  action: navigate
  target_id: detail/item        # resolves to a pattern in the catalog
  target_label: "Detail Screen" # human-readable fallback
```

### 3. `status_badge` carries semantic weight beyond a slot

The Chase "You've set up automatic payments ✓" is not just text — it has a type (`success`), and its presence implies a state machine (could also be `warning: payment due`, `alert: fraud detected`). The current `status_badge: {type: success | warning | alert}` in `content` is adequate for v1 but may need to move to `layout` if the badge's presence/absence affects structural layout.
