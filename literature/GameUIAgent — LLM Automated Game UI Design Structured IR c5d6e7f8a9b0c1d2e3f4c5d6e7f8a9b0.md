# GameUIAgent — LLM-Powered Framework for Automated Game UI Design with Structured Intermediate Representation

```
@misc{gameuiagent2026,
  title={{GameUIAgent}: An {LLM}-Powered Framework for Automated Game {UI} Design with Structured Intermediate Representation},
  year={2026},
  eprint={2603.14724},
  archivePrefix={arXiv}
}
```

**Quality grade**: B — systematic quantitative evaluation (110 cases, 3 LLMs, 3 templates); arXiv preprint, no confirmed venue.
**Stance on claim**: partial

# One Sentence
A six-stage neuro-symbolic pipeline translating natural language game UI descriptions into Figma designs via Design Spec JSON as a structured intermediate, evaluated across three UI templates.

---

# More Sentences
The pipeline combines LLM generation, deterministic post-processing, and a VLM-guided Reflection Controller for iterative self-correction. Design Spec JSON bridges natural language input and visual output. Evaluation across 110 test cases identified a "Quality Ceiling Effect" (reflection improvements plateau below quality thresholds) and a "Rendering-Evaluation Fidelity Principle."

---

# Key Points

### Design Spec JSON as intermediate
A JSON schema describing game UI layout and components. Domain-specific (game rarity tiers, game HUD conventions), not a general interaction design pattern library.

### UI templates used in evaluation
Three templates for three game UI rarity tiers — these are fixed domain templates, not a retrievable library of named semantic interaction patterns.

# Other Notes
- Most structurally similar to the PatternGenUI pipeline (JSON intermediate + templates), but the "patterns" are hardcoded game-domain templates, not a curated named library with retrieval.

# Take-Away
- Demonstrates the JSON-intermediate + template approach in a specific domain. PatternGenUI generalizes this to a curated retrievable library of named interaction design patterns across app domains.
