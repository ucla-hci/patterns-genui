# Deng — Do Code LLMs Understand Design Patterns?

```
@misc{deng2025codellmsdesignpatterns,
  title={Do Code {LLM}s Understand Design Patterns?},
  year={2025},
  eprint={2501.04835},
  archivePrefix={arXiv},
  note={ICSE 2025 Workshop}
}
```

**Quality grade**: B — systematic experiments across recognition, comprehension, and generation; ICSE workshop venue.
**Stance on claim**: not addressed

# One Sentence
Evaluates Code LLMs' ability to recognize, comprehend, and generate code with design patterns, finding that LLMs "fail to capture existing coding standards" and their biases significantly affect reliability.

---

# More Sentences
The study tests Code LLMs across three dimensions: pattern recognition, comprehension, and application in code generation. Results show Code LLMs frequently generate code that conflicts with project-required design patterns, requiring manual revision. The study targets software engineering design patterns (GoF), not interaction design patterns, but the finding supports external conditioning.

---

# Take-Away
- Motivating evidence: LLMs do not reliably internalize design patterns without external conditioning. Supports the argument that an explicit pattern intermediate (rather than relying on LLM implicit knowledge) is necessary for PatternGenUI.
