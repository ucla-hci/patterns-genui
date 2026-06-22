# AlignUI — A Method for Designing LLM-Generated UIs Aligned with User Preferences

```
@misc{alignui2026,
  title={{AlignUI}: A Method for Designing {LLM}-Generated {UI}s Aligned with User Preferences},
  year={2026},
  eprint={2601.17614},
  archivePrefix={arXiv}
}
```

**Quality grade**: B — large crowdsourced dataset (720 preferences, 50 users) + user study (72 users); arXiv preprint.
**Stance on claim**: not addressed

# One Sentence
Injects a crowdsourced user preference dataset (720 UI control preferences across eight image-editing tasks) into LLM reasoning to generate UIs aligned with general user preferences across multiple dimensions.

---

# More Sentences
AlignUI collects preference data from 50 users across eight image-editing tasks and injects this dataset into LLM prompts to guide generation. A user study with 72 additional users on six unseen tasks demonstrated that generated UIs closely align with multiple preference dimensions. The approach is pre-generation (preference injection) rather than mid-generation pattern conditioning.

---

# Key Points

### Preference dataset as context
The "intermediate" is a preference dataset injected into prompts. This is RAG-style injection of user preference data, not a curated library of named semantic design patterns.

# Take-Away
- Represents the user-preference-alignment approach to GenUI controllability. PatternGenUI addresses transparency and design knowledge rather than preference conformance. Not a close prior art for the novelty claim.
