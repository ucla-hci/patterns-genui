# Cao — Jelly: Generative and Malleable User Interfaces with Generative and Evolving Task-Driven Data Model

```
@inproceedings{cao2025jelly,
  title={Generative and Malleable User Interfaces with Generative and Evolving Task-Driven Data Model},
  author={Yining Cao and Peiling Jiang and Haijun Xia},
  booktitle={Proceedings of the 2025 CHI Conference on Human Factors in Computing Systems},
  year={2025},
  doi={10.1145/3706598.3713285}
}
```

**Quality grade**: A — CHI full paper, user study; sample is small (8 participants) but adequate for exploratory CHI work.
**Stance on claim**: partial

# One Sentence
Jelly generates and evolves task-driven data models (object-relational schemas + dependency graphs) in response to natural language prompts, using predefined UI patterns and rules to compose interfaces that end-users can malleably modify.

---

# More Sentences
The system interprets user prompts to generate a task-driven data model as a structured intermediate, then applies predefined UI patterns and composition rules to render an interface. Users can further modify the interface via natural language or direct manipulation. A user study with 8 participants found the interface intuitive and capable of adapting to evolving task requirements.

---

# Key Points

### Predefined UI patterns as composition rules
> "[The system] utilizes predefined UI patterns and rules that follow common design practices, ensuring consistency and usability."

These are composition constraints applied uniformly, not a named library of screen-type interaction patterns retrieved per-prompt.

### End-user vs. designer use case
Jelly targets end-users adapting interfaces to personal tasks, not designers controlling generation quality and transparency.

# Other Notes
- Highest-quality paper in the near-neighbor cluster (CHI full paper).
- The "predefined UI patterns" terminology is the closest to PatternGenUI's language — the paper must make the distinction explicit in related work.

# Take-Away
- Sharpest argument risk: Jelly uses "predefined UI patterns" at CHI 2025. Differentiator: Jelly's patterns are hardcoded composition rules; PatternGenUI's patterns are a curated named library (à la Borchers/Alexander) retrieved per-prompt and presented transparently to the designer.
