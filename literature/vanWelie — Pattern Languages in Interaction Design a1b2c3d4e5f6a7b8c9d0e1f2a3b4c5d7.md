# van Welie & van der Veer — Pattern Languages in Interaction Design: Structure and Organization

```
@inproceedings{vanWelie2003,
  author    = {van Welie, Martijn and van der Veer, Gerrit C.},
  title     = {Pattern Languages in Interaction Design: Structure and Organization},
  booktitle = {Proceedings of INTERACT 2003},
  year      = {2003},
  publisher = {IOS Press},
  note      = {Available: https://www.semanticscholar.org/paper/Pattern-Languages-in-Interaction-Design:-Structure-Welie-Veer/3b4118b996840e8da37426273ac00b09050ffaf7}
}
```

**Quality grade**: B — peer-reviewed IFIP conference, conceptual framework without formal empirical evaluation.
**survey** → **Approach type**: Foundational pattern language — hierarchical structure

# One Sentence
Proposes a top-down hierarchical organization for web UI pattern languages, distinguishing task, navigation, and widget levels.

---

# More Sentences
The paper argues that ad hoc pattern collections lack useful structure and proposes organizing interaction design patterns into a three-level hierarchy: high-level task patterns, mid-level navigation patterns, and low-level widget patterns. Van Welie also maintains a companion online catalog at welie.com/patterns with 80+ named web UI patterns. The work is notable for being both a theoretical framework and a practical catalog.

---

# Key Points

### Three-level hierarchy
> Task patterns → Navigation patterns → Widget patterns; each level is refined by the next.

### welie.com catalog
The accompanying online catalog (welie.com/patterns) provides named patterns like "Search," "Shopping Cart," "Breadcrumb," and "Wizard" with context/problem/solution structure.

# Other Notes
The catalog is web-specific; not validated empirically. Pattern relationships are specified informally. Complements Borchers' foundational argument by adding structural organization.

# Take-Away
- The most explicit attempt to organize an interaction pattern library hierarchically — directly relevant to PatternGenUI's retrieval layer design.
- Level-of-granularity dimension (task → screen → widget) is a key axis for PatternGenUI's pattern catalog.
