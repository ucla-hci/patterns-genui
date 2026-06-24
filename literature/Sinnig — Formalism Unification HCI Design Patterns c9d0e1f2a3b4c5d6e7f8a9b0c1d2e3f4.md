# Sinnig et al. — Bringing Formalism and Unification to HCI Design Patterns

```
@inproceedings{Sinnig2010,
  author    = {Sinnig, Daniel and Chalin, Patrice and Khendek, Ferhat},
  title     = {Bringing Formalism and Unification to Human-Computer Interaction Design Patterns},
  booktitle = {Proceedings of the 1st International Workshop on Pattern-Driven Engineering of Interactive Computing Systems (PDEICS '10)},
  year      = {2010},
  publisher = {ACM},
  doi       = {10.1145/1824749.1824754},
  url       = {https://dl.acm.org/doi/10.1145/1824749.1824754}
}
```

**Quality grade**: C — ACM workshop paper, system design with examples, no empirical evaluation.
**survey** → **Approach type**: Formal/computational pattern representation

# One Sentence
Introduces XPLML (eXtended Pattern Language Markup Language), a formal XML schema for HCI design patterns that captures UI primitives, relationships, and pattern content elements machine-readably.

---

# More Sentences
Sinnig et al. argue that existing prose pattern descriptions prevent computational tooling (discovery, enforcement, composition) and propose XPLML as a unifying formal schema. XPLML specifies: content elements (name, context, problem, solution), UI primitives (atomic UI building blocks), and inter-pattern relationships. The schema is illustrated with examples but not empirically evaluated. A follow-up paper (HCII 2013) uses XPLML to drive semi-automated UI generation.

---

# Key Points

### XPLML: first formal pattern schema for HCI
Bridges the gap between prose patterns and software tools. UI primitives are the "atoms" that solution descriptions reference.

### Enables: discovery, enforcement, composition
Formal spec allows pattern-matching algorithms, consistency checking, and compositional generation — all directly relevant to PatternMCP.

### 2013 follow-up: semi-automated UI generation
Sinnig's HCII 2013 paper uses XPLML specs to partly automate UI generation — the closest academic precursor to PatternGenUI's generation architecture.

# Other Notes
XPLML's scope is component/widget level, not screen level. PatternGenUI needs screen-level patterns. XPLML is a useful starting point but would need extension. Not empirically evaluated.

# Take-Away
- **Most directly relevant formalization work**: XPLML is the closest prior formal pattern spec; PatternGenUI's spec language should be explicitly differentiated from it (screen-level vs. widget-level; LLM-conditioned vs. template-matched).
- Cite as the formalization anchor in the related work section.
