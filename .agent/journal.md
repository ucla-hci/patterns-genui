# Augmenting GenUI with Design Patterns

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