# Literature Synthesis: Novelty of PatternGenUI

## Claim
Using interaction design patterns as a mid-generation conditioning layer improves the transparency and controllability of LM-based generative UI (code generation). Prior work either (1) improves pre-generation intent conveyance or (2) enables post-generation iteration; no prior work uses patterns as a structured intermediate that conditions the LM during generation itself.

---

## Verdict

```
VERDICT: partially addressed

CLOSEST PRIOR WORK:
1. Lu et al. (2023) "UI Layout Generation with LLMs Guided by UI Grammar" — uses grammar
   (context-free structural rules) as a structured intermediate to improve LLM UI layout
   controllability. Differentiator: grammar encodes parent-child layout structure (A→B
   rules); no curated pattern library, no retrieval, no named screen-type semantics.
   Workshop position paper, no user study.

2. Cao et al. (CHI 2025) "Jelly" — uses "predefined UI patterns and rules that follow
   common design practices" alongside a task-driven data model. Differentiator: Jelly's
   patterns are hardcoded composition rules applied uniformly, not a curated named library
   of interaction design patterns (à la Borchers/Alexander) retrieved per-prompt. End-user
   malleability use case, not designer-facing generation transparency.

3. Chen et al. (2025) "SpecifyUI" — uses SPEC, a structured parameterized hierarchical IR
   exposing UI elements as controllable parameters, extracted from visual UI references.
   Differentiator: SPEC is derived from visual screenshots per session; no curated named
   pattern library; encodes visual parameters, not semantic screen-type conventions.

WHAT'S NOVEL (the three-part combination no paper has):
- The intermediate is named interaction design patterns from a curated library — screen-type
  semantic conventions (e.g., "e-commerce category list") encoding design knowledge, not
  layout rules, visual parameters, or developer scaffolds.
- A retrieval step: prompt → matching pattern(s) from the library, giving the designer a
  choice among relevant conventions.
- Designer-facing transparency: the retrieved pattern is shown to and editable by the
  designer before generation proceeds — transparency at the semantic design level.
- Evaluation design: ablative (pattern vs. no-pattern, same LM) + designer study vs.
  professional SOTA tool. No adjacent paper uses this paired evaluation design.

WHAT'S UNSUPPORTED / RISKS:
- The structured-intermediate mechanism for LLM UI controllability is well-established
  (Lu 2023, Athena 2025, SpecifyUI 2025, GUIDE 2025, Jelly CHI 2025). The claim cannot
  be "structured intermediates help" — it must be "named design patterns are the right
  intermediate for design-knowledge transparency, and here is evidence."
- Jelly (CHI 2025) uses "predefined UI patterns" language — this overlap must be
  explicitly addressed in the related work section.
- The novelty argument is strong but hinges on a clear definition of interaction design
  patterns (Borchers 2000; Alexander's pattern language) vs. grammar rules, SPEC
  parameters, and composition rules.

RECOMMENDED NEXT STEPS:
- Write a tight one-paragraph differentiator for the intro distinguishing PatternGenUI from
  the structured-IR cluster (Lu, Athena, SpecifyUI, GUIDE, Jelly) — the key move is:
  "all prior work uses structural/visual/scaffold intermediates; we use a semantic design-
  knowledge intermediate (named patterns) that is retrievable, inspectable, and editable."
- The related work section should have a dedicated subsection on structured intermediates
  for GenUI, clearly placing each neighbor.
- Jelly needs a sentence of direct differentiation in related work.
```

---

## Literature Matrix

| Source | Structured IR | Named pattern library | Retrieval step | Designer visibility | Formal evaluation | Grade |
|--------|:---:|:---:|:---:|:---:|:---:|:---:|
| Lu et al. (2023) — UI Grammar | ✓ | ✗ | ✗ | partial | ✗ | C |
| Jiang et al. (IUI 2025) — Athena | ✓ | ✗ | ✗ | ✓ (dev) | ✓ | B |
| Chen et al. (2025) — SpecifyUI | ✓ | ✗ | ✗ | ✓ | ✓ | B |
| Kolthoff et al. (ICSE 2025) — GUIDE | ✓ | ✗ (components) | ✓ (components) | partial | partial | B |
| GameUIAgent (2026) | ✓ | ✗ (templates) | ✗ | ✗ | ✓ | B |
| Cao et al. (CHI 2025) — Jelly | ✓ | ✗ (rules) | ✗ | ✓ | ✓ | A |
| Li et al. (TOCHI 2025) — PrototypeFlow | partial | ✗ | ✗ | ✓ | ✓ | A |
| AlignUI (2026) | ✗ | ✗ | ✓ (prefs) | ✗ | ✓ | B |
| Generative Interfaces (2025) | ✓ | ✗ | ✗ | ✗ | ✓ | B |
| DesignerlyLoop (2025) | ✗ | ✗ | ✗ | ✓ | ✓ | B |
| Deng et al. (ICSE 2025) — Code LLMs+Patterns | ✗ | ✗ | ✗ | ✗ | ✓ | B |
| AI-Inspired UI Design (2024) | ✗ | ✗ | ✗ | ✗ | ✗ | D |
| **PatternGenUI (proposed)** | **✓** | **✓** | **✓** | **✓** | **✓ (planned)** | — |

**Convergence**: Moderate — multiple A/B papers confirm adjacent mechanisms; no direct contradiction; the specific three-part combination is unoccupied.

---

## Contradictions & Tensions

| Paper A | Paper B | Assessment | Status |
|---------|---------|------------|--------|
| Jelly (CHI 2025) uses "predefined UI patterns" | PatternGenUI claims no prior work uses patterns as intermediate | conditional_difference — Jelly's patterns are composition rules, not a named retrievable library | resolved by definition |
| Structured-IR approach well-established (Lu, Athena, SpecifyUI, GUIDE, Jelly) | PatternGenUI claims novelty of mid-generation conditioning | conditional_difference — prior work uses structural/visual/scaffold IRs; PatternGenUI claims novelty of semantic design-knowledge IR | requires clear argument in paper |

---

## Knowledge Gaps

| Gap | Type | Why it matters |
|-----|------|----------------|
| No paper evaluates named interaction design patterns (Borchers/Alexander) as a conditioning intermediate for LLM UI generation | Theoretical + Empirical | Core of the novelty claim |
| No paper uses a curated, retrievable library of screen-level semantic patterns for GenUI | Methodological | PatternGenUI's specific contribution |
| No adjacent paper compares to a professional design tool (e.g., Figma, Adobe XD) in designer study | Empirical | PatternGenUI's evaluation is more ecologically valid than all neighbors |
| Code LLMs do not reliably apply design patterns without external conditioning (Deng 2025) | Empirical | Supports the claim that explicit pattern intermediates are necessary |

---

## Anchor Papers

1. **Lu et al. (2023)** — nearest neighbor on mechanism; must be directly addressed in related work; sets up the grammar vs. patterns distinction.
2. **Cao et al. (CHI 2025) — Jelly** — uses "UI patterns" language at CHI; highest argument risk; must be explicitly differentiated.
3. **Chen et al. (2025) — SpecifyUI** — strongest adjacent designer evaluation (16 professional designers); benchmarks the evaluation bar.
4. **Li et al. (TOCHI 2025) — PrototypeFlow** — highest-rigor paper in the pre+post camps; most representative of the two prior camps named in the intro.
5. **Borchers (2000) + Folmer (2015)** — foundational on what interaction design patterns are; essential for defining the intermediate.
