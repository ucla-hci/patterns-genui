# Chen — SpecifyUI: Supporting Iterative UI Design Intent Expression through Structured Specifications

```
@misc{chen2025specifyui,
  title={SpecifyUI: Supporting Iterative {UI} Design Intent Expression through Structured Specifications and Generative {AI}},
  author={Yunnong Chen and Chengwei Shi and Liuqing Chen},
  year={2025},
  eprint={2509.07334},
  archivePrefix={arXiv}
}
```

**Quality grade**: B — strong user study (16 professional designers); arXiv preprint, no confirmed venue.
**Stance on claim**: partial

# One Sentence
Introduces SPEC — a structured, parameterized, hierarchical intermediate representation exposing UI elements as controllable parameters — extracted from visual UI references via region segmentation and VLMs, used to guide multi-agent UI generation.

---

# More Sentences
SPEC is extracted from existing UI screenshots by segmenting regions (Region, Style, Layout fields) and encoding them as controllable parameters. A multi-agent generator renders SPEC into designs, supporting targeted edits at global, regional, and component levels. A user study with 16 professional designers showed SpecifyUI significantly outperformed Stitch on intent alignment, design quality, controllability, and overall experience.

---

# Key Points

### SPEC: visual parameter IR
SPEC is derived from visual references — it parameterizes layout regions, styles, and compositions. It is not a library of named semantic screen types; it is a structural-visual parameterization extracted case-by-case from references.

### Strong designer evaluation
16 professional designers, comparison to Stitch. This is the most rigorous designer evaluation in the adjacent literature and sets a benchmark for PatternGenUI's own evaluation.

# Other Notes
- No retrieval step from a curated named library — SPEC is extracted from visual references per session, not matched from a reusable pattern catalog.
- The intermediate encodes element parameters, not "what kind of screen this is and why."

# Take-Away
- Second-sharpest argument risk. PatternGenUI must distinguish: SpecifyUI extracts a visual IR from references; PatternGenUI retrieves a semantic IR (named pattern) from a curated library. The semantic layer (design conventions and intent) is the claimed differentiator.
