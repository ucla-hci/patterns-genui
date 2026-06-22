# Li — PrototypeFlow: Towards Human-AI Synergy in UI Design (Supporting Iterative Generation with LLMs)

```
@article{li2025prototypeflow,
  title={Towards Human--{AI} Synergy in {UI} Design: Supporting Iterative Generation with {LLM}s},
  author={[Li et al.]},
  journal={ACM Transactions on Computer-Human Interaction},
  year={2025},
  doi={10.1145/3773035}
}
```

**Quality grade**: A — TOCHI journal, multi-method study (formative + user study); gold standard HCI venue.
**Stance on claim**: partial

# One Sentence
PrototypeFlow is a human-centered system for iterative UI design with LLMs, combining a theme design module (pre-generation intent clarification) with component-level generation and direct editing of intermediate results.

---

# More Sentences
A formative study identified requirements for supporting iterative design with generative tools. PrototypeFlow addresses both pre-generation (prompt enhancement, theme clarification) and post-generation (direct editing of component-level outputs). Experiments and user studies confirmed effectiveness; published in TOCHI, the flagship HCI journal.

---

# Key Points

### Theme design module (pre-generation)
Uses an LLM-based controller for prompt enhancement and theme-level intent clarification — this is the pre-generation camp cited in PatternGenUI's intro.

### Iterative editing (post/mid-generation)
Designers can edit intermediate results (component specifications) — partial overlap with PatternGenUI's designer-visibility goal, but no named pattern library or retrieval.

# Other Notes
- Highest-rigor paper in the pre-generation camp; published TOCHI.
- Addresses a similar transparency goal (making intermediate results visible and editable) but through a different mechanism (theme clarification + component editing vs. semantic pattern retrieval).

# Take-Away
- PatternGenUI should cite PrototypeFlow as the strongest prior work in the pre+post camps. The key differentiator: PrototypeFlow clarifies intent before/after; PatternGenUI uses named design patterns as a semantic intermediate during generation.
