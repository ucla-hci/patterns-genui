# Kolthoff — GUIDE: LLM-Driven GUI Generation Decomposition for Automated Prototyping

```
@inproceedings{kolthoff2025guide,
  title={{GUIDE}: {LLM}-Driven {GUI} Generation Decomposition for Automated Prototyping},
  author={Kristian Kolthoff and Felix Kretzer and Christian Bartelt and Alexander Maedche and Simone Paolo Ponzetto},
  booktitle={Proceedings of the 47th International Conference on Software Engineering (ICSE)},
  year={2025},
  eprint={2502.21068},
  archivePrefix={arXiv}
}
```

**Quality grade**: B — ICSE peer-reviewed; evaluation described as preliminary.
**Stance on claim**: partial

# One Sentence
Decomposes high-level GUI descriptions into fine-grained requirements, then uses RAG over a Material Design component library to generate editable Figma prototypes.

---

# More Sentences
GUIDE follows a two-stage pipeline: (1) decompose user description into fine-grained GUI features (e.g., "header section," "profile section"), then (2) RAG-retrieve relevant Material Design component combinations for each feature. The result is rendered directly in Figma with editing support. The evaluation is preliminary; no detailed user study metrics are reported.

---

# Key Points

### Decomposition as intermediate
The intermediate is a fine-grained feature list, not a named screen-type pattern. The RAG step retrieves components, not design patterns.

### Component library ≠ pattern library
Material Design components are atomic UI elements (buttons, cards, chips); interaction design patterns are screen-level semantic conventions (e.g., "horizontal category list," "settings hierarchy").

# Other Notes
- Most adjacent to PatternGenUI on the RAG-over-library dimension, but the library is a component library (atoms), not a pattern library (screen-level conventions).

# Take-Away
- Confirms that RAG-over-library for UI generation is an active approach. PatternGenUI lifts the abstraction level: retrieve screen-level semantic patterns rather than atomic components.
