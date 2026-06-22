# Research Passport

## Topic
Using interaction design patterns as a mid-generation conditioning layer to improve the transparency and controllability of LM-based generative UI (GenUI).

## Stages
| Stage | Status |
|-------|--------|
| 0 · Intake              | done |
| 1 · Novelty verification | done |

## Notes
- Primary claim is **empirical**: patterns improve GenUI quality/controllability (not a pure systems paper)
- Target venues: CHI, UIST; NLP/AI venues possible (ACL, EMNLP, NeurIPS)
- System (PatternMCP + PatternGenUI) not yet built — paper at design/proposal stage
- Two prior approaches in the intro: (1) pre-generation intent conveyance, (2) post-generation slot-based iteration; proposed approach is complementary mid-generation conditioning
- Evaluation plan: ablative (pattern vs. no-pattern, same LM) + designer study vs. SOTA tool
- Existing literature covers patterns theory (Borchers 2000, Folmer 2015); LM/GenUI side not yet seeded
- Novelty search scope: (1) HCI — patterns + UI generation, transparency/control in GenUI; (2) NLP/AI — LM-based UI code generation, code generation conditioned on structured intermediates
