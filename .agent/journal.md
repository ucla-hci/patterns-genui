# Augmenting GenUI with Design Patterns

## 6/30/2026

```
- [] try to use the patterns to gen ui code
- [] experimental plan to compare ui quality w/ vs. w/o patterns
```

## 6/24/2026
```
- [x] survey papers on design patterns similar to Borchers's and Folmer's
- [x] grounded in the found literature, brainstorm a specification language to define UX/UI design pattern--using mobile app as a starting point, each pattern can focus on a mobile screen: start by selecting a specific example mobile screen and try to describe it using a pattern language.
```

### THOUGHTS
- a pattern atomically specifies the design of a screen
- a designer might try to generate all screens for an app at once; then it's the LLM's job to decide what patterns of screens to use
- maybe: a pattern can contain knowledge about which other types of screens it can navigate to

### Survey Papers on Design Patterns

**Approach.** Two-pass search (dry-run → full). 14 papers across 7 approach types. 12 new note files written. Synthesis report: `literature/_approaches-design-pattern-catalogs.md`.

**Approach taxonomy (7 types):**
1. **Foundational pattern language** — Borchers 2000; van Welie 2003 — Alexander template imported to HCI; hierarchical structure
2. **Practitioner catalog — general** — Tidwell 2005/2019; van Duyne 2002 — expert-curated, visual, not peer-reviewed
3. **Practitioner catalog — mobile** — Neil 2014; Nilsson 2009 — mobile-specific, 90+ patterns (Neil), screen-level
4. **Domain-specific catalog** — Landay & Borriello 2003; Chung 2004; Folmer 2015 — ubicomp, game/accessibility
5. **Critical review / SLR** — Dearden & Finlay 2006; Punchoojit 2017; Seffah 2010 — maps field, identifies eval gap
6. **Formal/computational representation** — Sinnig 2010 — XPLML schema, widget-level, no eval
7. **Empirically-derived** — Nguyen 2018 — deep learning on RICO (72k screens), implicit patterns

**Key findings for PatternGenUI:**
- Every catalog is expert-curated except Nguyen (ML-induced). No one mines named patterns from data and makes them designer-facing.
- Only one controlled evaluation study exists: Chung et al. (2004, DIS) — patterns help process (design space coverage) but not output quality. Never replicated.
- Mobile is the most under-standardized domain: Neil (2014) is the closest anchor but a practitioner book, not peer-reviewed.
- The untried combination PatternGenUI occupies: mobile-first + structured machine-readable spec + designer-facing retrieval + generation conditioning.
- Sinnig (2010) XPLML is the closest formalization work — differentiate: screen-level (ours) vs. widget-level (XPLML); LLM-conditioned (ours) vs. template-matched (XPLML).

**Anchor papers (must cite):** Borchers 2000 · Dearden & Finlay 2006 · Neil 2014 · Chung 2004 · Sinnig 2010 · Nguyen 2018 · Tidwell 2019

### Pattern Specification Language

**Approach.** Four drafts (v1→v4) driven by two application tests: DoorDash (discover) and Chase (monitor). Final spec: `.agent/drafts/pattern-spec-language-brainstorm-v4.md`.

**Format.** YAML frontmatter (machine-readable, injected into LLM prompt) + Markdown body (designer-facing, shown in UI only). No custom tooling required.

**Unit.** One screen per pattern, named by user intent. Category enum from mobile-ui-taxonomy: onboard · configure · browse · search · discover · detail · create · transact · monitor.

**Six design principles codified:**
1. Pattern = intent × layout topology — same pattern applies across product domains (Chase, Robinhood, Apple Health, Asana all instantiate `monitor/grouped-items`)
2. `layout` uses domain-neutral component names; domain-specific terms belong in `content` + `examples`
3. `[]` suffix marks repeating structural units (e.g., `item_group[]`, `result_card[]`)
4. `use_when`/`not_when` = selection conditions (machine-readable); `rationale` = design reasoning (designer UI only) — distinct purposes
5. `role: action | filter` required on header chip rows — semantically critical for LLM event handler generation
6. `interactions` is a typed schema `{component, on, action: navigate|effect, target_label, target_id?, effect?}` — enables navigation graph queries

**Open questions:** typed `target_id` references (needs catalog first); `role` required vs optional; variant storage (inline vs sibling files).

**Application test files:** `.agent/drafts/pattern-spec-application-test.md` (DoorDash) · `.agent/drafts/pattern-spec-application-test-2-chase.md` (Chase)


## 6/22/2026

```
[x] familiarize yourself with this project
[x] verify the novelty of this project
```

### Familiarize Yourself With This Project

**What it is.** "Augmenting GenUI with Design Patterns" proposes using interaction design patterns as a mid-generation conditioning layer to improve transparency and control in LM-based UI generation.

**Core claim.** Empirical: patterns improve GenUI quality and controllability. The system artifact (PatternMCP + PatternGenUI front-end) is not yet built.

**Positioning.** Complements two existing camps: (1) pre-generation intent conveyance and (2) post-generation slot-based iteration. This approach operates during generation.

**Evaluation plan.** Ablative study (pattern vs. no-pattern, same LM) + designer study vs. a state-of-the-art tool.

**Existing literature.** Covers interaction design pattern theory (Borchers 2000; Folmer 2015). The LM/GenUI side is not yet seeded — novelty verification is the next step.

**Target venues.** CHI, UIST; NLP/AI venues (ACL, EMNLP, NeurIPS) also in scope.

### Verify the Novelty of This Project

**Verdict.** Partially addressed — the structured-intermediate mechanism for LLM UI controllability is well-established, but the specific combination (named design pattern library → retrieval → designer-visible conditioning) has not been done.

**Nearest neighbors (the structured-IR cluster):**
- **Lu et al. (2023) — UI Grammar** (ICML workshop): grammar encodes parent-child layout structure; no library, no retrieval, no named screen types. Grade: C.
- **Cao et al. (CHI 2025) — Jelly**: uses "predefined UI patterns and rules" as composition constraints alongside a task-driven data model. Highest argument risk — must be explicitly differentiated in related work. Grade: A.
- **Chen et al. (2025) — SpecifyUI**: SPEC as a visual-parameter IR extracted from UI screenshots; 16-designer user study sets the evaluation bar. Grade: B.
- **Jiang et al. (IUI 2025) — Athena**: storyboard + data model + GUI skeletons for iterative app generation; developer-scaffold focus. Grade: B.
- **Kolthoff et al. (ICSE 2025) — GUIDE**: two-stage decomposition + RAG over Material Design component library; components ≠ screen-level patterns. Grade: B.
- **GameUIAgent (2026)**: Design Spec JSON + 3 game UI templates; domain-specific, no retrievable library. Grade: B.

**Other relevant work:**
- **Li et al. (TOCHI 2025) — PrototypeFlow**: strongest prior work in the pre+post camps; multi-method study; cite as the representative of the two intro camps. Grade: A.
- **Deng et al. (ICSE 2025)**: LLMs fail to apply design patterns without external conditioning — motivates explicit pattern intermediates. Grade: B.

**What's novel (unoccupied combination):** curated named pattern library + retrieval step + designer-facing inspection/edit before generation + ablative × designer evaluation vs. SOTA tool.

**Key risk.** Jelly's "predefined UI patterns" wording at CHI 2025 is the sharpest argument risk. Differentiator: Jelly's patterns are hardcoded composition rules; PatternGenUI's are named interaction design patterns (Borchers/Alexander) retrieved per-prompt.

**Files written.** 12 note files + `_synthesis-patterns-genui-novelty.md` in `literature/`.