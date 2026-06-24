# Landay & Borriello — Design Patterns for Ubiquitous Computing

```
@article{Landay2003,
  author  = {Landay, James A. and Borriello, Gaetano},
  title   = {Design Patterns for Ubiquitous Computing},
  journal = {Computer},
  volume  = {36},
  number  = {8},
  pages   = {93--95},
  year    = {2003},
  publisher = {IEEE},
  doi     = {10.1109/MC.2003.1220589},
  url     = {https://dl.acm.org/doi/abs/10.1109/MC.2003.1220589}
}
```

**Quality grade**: B — IEEE Computer (flagship, peer-reviewed magazine), position paper with illustrative examples, no formal eval.
**survey** → **Approach type**: Domain-specific catalog — ubicomp domain

# One Sentence
Proposes that Alexander-style design patterns are the right mechanism for capturing ubicomp design knowledge, naming seven initial patterns (e.g., "Context-Sensitive I/O," "Follow-Me Display").

---

# More Sentences
The paper argues that ubicomp systems face the same structured-yet-informal knowledge transfer problem that motivated GoF and Borchers — and that patterns are the right solution. Seven named patterns are introduced at a high level, spanning physical-virtual associations, context awareness, and cross-device continuity. The paper is more a call to action than a complete catalog; it directly spawned the Chung et al. (2004) empirical evaluation work.

---

# Key Points

### Seven named ubicomp patterns
Context-Sensitive I/O, Physical-Virtual Associations, Global Data, Proxies for Devices, Follow-Me Display, Appropriate Levels of Attention, Anticipation.

### Cross-device / multi-surface scope
These patterns operate at a level above single-screen — relevant to multi-modal and ambient UIs, less relevant to single mobile-screen focus.

# Other Notes
Useful precedent for how to launch a domain-specific pattern catalog with a small set of seed patterns before full validation. Companion to Chung et al. (2004) which evaluates the patterns empirically.

# Take-Away
- Demonstrates the pattern of "seed catalog + evaluation study" — a valid research design for PatternGenUI's contribution.
- Cross-device patterns are out of scope for PatternGenUI's mobile-screen focus but useful negative-case contrast.
