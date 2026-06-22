# Jiang — Athena: Intermediate Representations for Iterative Scaffolded App Generation

```
@inproceedings{jiang2025athena,
  title={Athena: Intermediate Representations for Iterative Scaffolded App Generation with an {LLM}},
  author={Jiang, [et al.]},
  booktitle={Proceedings of the 31st International Conference on Intelligent User Interfaces},
  year={2025},
  doi={10.1145/3742413.3789133},
  note={arXiv:2508.20263, Apple Machine Learning Research}
}
```

**Quality grade**: B — IUI peer-reviewed, user study with preference data; sample size not fully specified.
**Stance on claim**: partial

# One Sentence
Athena uses three shared intermediate representations — app storyboard, data model, and GUI skeletons — to scaffold iterative LLM-based app generation, reducing errors and improving developer preference over a chatbot baseline.

---

# More Sentences
The three IRs structure the LLM's code generation process: the storyboard maps screen navigation, the data model specifies entity structure, and GUI skeletons provide screen-level structural templates. Users collaboratively assemble these IRs via chat, and 75% preferred Athena over a standard chatbot prototype tool. The focus is developer scaffolding for multi-screen app generation, not designer-facing transparency over semantic design choices.

---

# Key Points

### The three intermediates
Storyboard (navigation flow) + Data model (entities/relationships) + GUI skeletons (per-screen structure). None encode design conventions or screen-type semantics.

### Scaffolding vs. transparency
The IRs scaffold code organization and reduce LLM errors; they do not expose design-knowledge rationale (why this screen type, what conventions apply).

# Other Notes
- Apple-affiliated research (disclosed).
- Nearest neighbor on the IR-for-UI-generation pattern, but intermediates are developer scaffolds (architectural + structural), not a curated library of named interaction design patterns.

# Take-Away
- Validates the structured-IR approach empirically (IUI, 75% preference). PatternGenUI extends this with a semantic design-knowledge layer: named patterns from a curated library retrieved per prompt, visible and editable by the designer.
