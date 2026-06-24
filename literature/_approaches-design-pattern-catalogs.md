# Approach Synthesis: Defining and Cataloging Named UX/UI/HCI Interaction Design Patterns

## Problem
How has prior work approached defining, organizing, and cataloging named interaction design patterns for UX/UI/HCI — excluding component libraries and ML-based generation?

---

## Approach Taxonomy

| Approach type | Representative papers | Core mechanism | Strengths | Limitations |
|---|---|---|---|---|
| Foundational pattern language | Borchers 2000; van Welie & van der Veer 2003 | Alexander template (context/problem/solution/forces) adapted to HCI; hierarchical cross-references | Principled structure; interdisciplinary lingua franca; level-of-granularity hierarchy | Informal prose; no empirical validation; no standard template |
| Practitioner catalog — general | Tidwell 2005/2019; van Duyne et al. 2002/2007 | Expert-curated named patterns with annotated screenshots, organized by interaction concern or site type | Immediately actionable; widely adopted by designers | Not peer-reviewed; no empirical backing; no machine-readable structure |
| Practitioner catalog — mobile | Neil 2014; Nilsson 2009 | Named patterns specific to smartphone/mobile interactions, illustrated with real app screenshots | Domain-appropriate; comprehensive (Neil: 90+ patterns, 11 categories) | Predates touch-paradigm maturity (Nilsson); not validated; currency gap |
| Domain-specific catalog | Landay & Borriello 2003; Chung et al. 2004; Folmer 2015 | Named patterns for a specific domain (ubicomp, game/accessibility) derived from domain experience | Deep domain fit; Chung et al. provides the only controlled evaluation | Narrow scope; cross-device/game patterns not mobile-screen; cross-domain alignment absent |
| Critical review / SLR | Dearden & Finlay 2006; Punchoojit & Hongwarittorrn 2017; Seffah 2010 | Systematic mapping and critique of existing catalogs | Maps the field; identifies gaps (eval, standardization, mobile); frames historical trajectory | No new patterns; secondary literature; results quickly dated |
| Formal/computational representation | Sinnig et al. 2010 | Machine-readable XML schema (XPLML) capturing content elements, UI primitives, and inter-pattern relationships | Enables tooling (discovery, enforcement, composition); first formal spec | Widget-level only; no evaluation; not screen-level; not designer-facing |
| Empirically-derived catalog | Nguyen et al. 2018 | Deep learning (RNN/GAN) on 72k+ real app screens (RICO dataset) to induce structural regularities | Grounded in real-world apps at scale; empirically validates that named patterns are real | ML-based discovery; produces implicit patterns not a named designer-facing catalog; no retrieval interface |

---

## Dimension Map

| Paper | Domain | Granularity | Formalization | Derivation | Designer-facing? | Formally evaluated? |
|---|---|---|---|---|---|---|
| Borchers 2000 | General HCI | Screen + flow | Informal prose | Expert | Yes | No |
| van Welie 2003 | Web | Widget → screen | Hierarchical prose | Expert | Yes | No |
| van Duyne 2002/2007 | Web | Screen → site | Prose | Expert | Yes | Partial |
| Landay & Borriello 2003 | Ubicomp | System/cross-device | Prose | Expert | Yes | No |
| Chung et al. 2004 | Ubicomp | System/cross-device | Prose pre-pattern | Expert + eval | Yes | **Yes** (n=32 designers) |
| Dearden & Finlay 2006 | General HCI | All | Review | Literature | — | Survey |
| Nilsson 2009 | Mobile | Screen | Structured prose | Expert | Yes | No |
| Tidwell 2005/2019 | General/Web/Mobile | Widget → screen | Prose + visual | Expert | Yes | No |
| Seffah 2010 | General HCI | All | Historical narrative | Literature | — | Survey |
| Sinnig 2010 | General HCI | Widget | XML schema | Expert | **No** (tool-facing) | No |
| Neil 2014 | Mobile | Screen | Prose + visual gallery | Expert | Yes | No |
| Punchoojit 2017 | Mobile | Screen | SLR | Literature | — | Survey |
| Nguyen 2018 | Mobile | Screen | Implicit model weights | **Empirical (ML)** | No | Quantitative (reconstruction) |
| Folmer 2015 | Game + General | Screen | Web article | Expert | Yes | No |

---

## Evolution

**2000–2003 — Import phase.** Borchers (DIS 2000) transplants Alexander's pattern language into HCI, framing patterns as interdisciplinary vocabulary. Van Welie adds hierarchical structure (widget → screen → task). Landay & Borriello extend to ubicomp. The dominant question: *what is a pattern and how should it be structured?*

**2002–2007 — Practitioner takeover.** Academic pattern papers are eclipsed by practitioner books (van Duyne 2002; Tidwell 2005) that sacrifice rigor for accessibility. These books become the actual design artifacts used in industry. The academic framing of "pattern language" gives way to "pattern catalog."

**2004 — The one evaluation study.** Chung et al. (DIS 2004) conduct the only controlled study of whether patterns help designers. They do — for process (design space coverage, communication) but not significantly for output quality. This finding is never replicated.

**2006 — The critique lands.** Dearden & Finlay's critical review finds almost no evaluation in ~40 projects. The field acknowledges the gap but does not close it.

**2009–2014 — Mobile specialization.** Post-iPhone, mobile-specific catalogs emerge: Nilsson (2009, academic) then Neil (2014, practitioner). Touch interaction is acknowledged but patterns remain largely adapted from pre-touch design knowledge.

**2010 — Two responses to stagnation.** (a) Seffah calls for engineering-grade formalization; (b) Sinnig delivers the first formal schema (XPLML). Neither is adopted broadly — XPLML is widget-level and evaluates nothing.

**2017–2018 — Empirical turn.** Punchoojit's SLR confirms mobile patterns are real but unstandardized. Nguyen et al. use deep learning to induce patterns from 72k+ screens — proving patterns are empirically real, not just expert taxonomy. No designer-facing artifact is produced.

**2024–2026 — LLM generation era.** GenUI tools emerge (Jelly, SpecifyUI, Athena, GUIDE) but none integrate retrievable named pattern libraries. The empirical gap and the formalization gap remain open.

---

## Tradeoffs

| Tension | Trade-off |
|---|---|
| Accessibility vs. rigor | Practitioner books are used; academic papers are rigorous. No work bridges both. |
| Breadth vs. depth | General catalogs (Tidwell: 100+ patterns) vs. domain catalogs (Neil: 90 mobile-specific). Depth requires domain narrowing. |
| Expert authority vs. empirical grounding | All catalogs except Nguyen are expert-curated. Empirical grounding (RICO-scale) produces patterns too implicit for designer use. |
| Prose readability vs. computational usability | Prose patterns are designer-friendly (Tidwell); formal specs (XPLML) are tool-friendly. No existing work achieves both. |
| Pattern as static description vs. active conditioning | Every prior catalog treats patterns as reference artifacts. None uses patterns as live conditioning signals during generation. |

---

## Untried Combinations / Open Space

| Empty cell | What it would look like | Relevance to PatternGenUI |
|---|---|---|
| Mobile-first + structured machine-readable spec | A JSON/YAML schema for screen-level mobile patterns, dual-readable by designers and LLMs | **PatternGenUI's primary contribution** |
| Named designer-facing catalog derived empirically (not ML weights) | RICO-grounded patterns that are also named, described, and retrievable | Possible future extension using RICO + LLM extraction |
| Controlled evaluation of pattern utility for LLM generation | Ablative study: same LLM, with vs. without pattern conditioning, measuring UI quality and designer satisfaction | **PatternGenUI's evaluation design** |
| Cross-domain pattern alignment | Are "Wizard" (web), "Onboarding" (mobile), and "Setup" (ubicomp) the same pattern? No work has aligned catalogs across domains | Future work for PatternGenUI after mobile catalog is established |
| Pattern retrieval interface for designers | A search/browse UI over a formal pattern catalog, letting designers inspect and edit patterns before generation | PatternGenUI's front-end design |

---

## Anchor Papers

1. **Borchers (2000)** — foundational statement; establishes the Alexander template and interdisciplinary vocabulary framing. Must cite.
2. **Dearden & Finlay (2006)** — authoritative critique; establishes the evaluation gap that PatternGenUI's study addresses. Must cite.
3. **Neil (2014)** — closest mobile pattern catalog; the baseline PatternGenUI's library should be reconciled against.
4. **Chung et al. (2004)** — only controlled evaluation study; the methodological precedent for PatternGenUI's ablative study.
5. **Sinnig et al. (2010)** — closest formalization work; differentiate PatternGenUI's screen-level LLM-conditioned spec from XPLML's widget-level template-matched schema.
6. **Nguyen et al. (2018)** — empirical validation that named mobile patterns are real; supports PatternGenUI's assumption that patterns are meaningful conditioning signals.
7. **Tidwell (2005/2019)** — practitioner gold standard for pattern readability; PatternGenUI's pattern descriptions should match this usability bar.
