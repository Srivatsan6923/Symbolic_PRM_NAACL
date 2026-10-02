# SymRM pilot vs the paper, and what v5 takes from it

Sources read: the SymRM Gate 2a dashboard, the Phase 3 report and the dataset
audit page, plus the second reviewer's recommendations. Nothing was re-run.

## Is the pilot a better paper than the draft?

No as a standalone, yes as a source of three ideas. It asks the same question
with a smaller and weaker test bed (10 rules, 300 scenarios, 3 far-OOD rules,
labels of "most appropriate prescription" that no rule establishes), no method
beyond a preference loss, and no step-level labels. What it has that the draft
lacked is listed first below.

## Taken into v5

| From the pilot / reviewer | Where in v5 |
|---|---|
| Near-miss condition (family-history allergy, value near a threshold) | Core of the paper: triplets = base, flip, near-miss; numeric and applicability near-misses (other subject, negated, past) |
| Finding that a keyword heuristic gets 100% on flips and 0% on near-misses | Proposition 1, second part; shortcut-baseline table (App. F) |
| Medication decisions as the motivating task | "Constraint rules" as a second kind of rule beside calculators; Figure 1 uses the allergy example |
| Neutral, length-matched candidate orders; raw-value fields in every case | Construction rules for constraint rules (App. B) |
| Preference between two candidates (Bradley-Terry view) | Metrics are threshold-free preferences: reversal, unnecessary reversal, triplet accuracy |
| Reviewer: flip-rate difference rewards any change | Triplet accuracy with component rates replaces Delta-flip |
| Reviewer: applicability fields in the ledger | Ledger adds subject, status, time; value-only ledger is an ablation |
| Reviewer: compare against paraphrase baselines (Compared to What?) | Presentation edits as the baseline for reversal |
| Reviewer: far-OOD set already used for decisions | Analysis plan: test triplets generated after the design is frozen |
| Dataset audit: CareQA has no overlap with MedQA | CareQA is the second MCQ endpoint and a source of key pairs |
| Dataset audit: ReMedQA's MedQA part lies inside MedQA test | Stated in the overlap audit; ReMedQA is not used |
| Phase 3 lesson: redundant reward terms under group-standardised advantages | GRPO protocol in App. G checks collinearity and a small-subset fit first |

## Not taken, and why

| Item | Reason |
|---|---|
| "Most appropriate prescription" labels (v0-unreviewed) | A contraindication rule does not establish that the alternative order is optimal (e.g. metformin 1000 mg twice daily as a first dose). v5 labels are correctness relative to a stated rule. |
| The 900-item set as test data | Too few rules for a transfer claim; it has already steered design decisions. Reuse the generator, regenerate under the stated-rule framing. |
| "BoW at chance, so zero leakage" | One classifier at chance does not certify a dataset. v5 reports several shortcut scorers and makes no such claim. |
| Phase 3 result (73.29% vs 73.52%, p = 0.82) | Different object (policy, answer-format perturbations) and uninformative: NOTA reward equals the correctness reward, so the sum gives the same standardised advantage; correctness-only training left MCQ accuracy at 56.50%, i.e. 60 steps changed nothing measurable. |
| ReMedQA as an evaluation tier (reviewer) | It measures answer-format stability of policies, a different axis; our claims are stated in content, not option letters. |
| Demoting calculators to an optional appendix (reviewer) | They plus constraint rules are the only exact tier and the only source of enough rules for a structural split. |
| Contradictory-ledger objection (reviewer) | Already resolved in v3: the judge never sees the case. |
| Presentation-consistency loss term (reviewer) | Presentation edits are training data; no extra loss is needed. |
| "MedPerturb" as a name | A published dataset already has it. The paper names no benchmark. |

## Points where the reviewer is right and v5 follows

* Constraint claims must be stated-rule compliance, not clinical optimality.
* Near-misses may change confidence; only the ranking must hold. v5 requires
  d > 0 on near-misses and no score equality.
* Report component rates, not only the aggregate.
* Cluster by scenario and rule in resampling; hold out structures, not words.

## Practical note

The pilot ran on an 8 GB laptop GPU with Qwen2.5-1.5B and Qwen3-4B (4-bit).
The paper's plan (8B backbone, 16 factorial cells x 5 seeds, API judges) needs
far more. The backbone is the macro \bb in main.tex. If the laptop is the real
budget: Qwen3-4B, 3 seeds, the 2 x 2 corner of the factorial (blocks vs
triplets; verdict vs applicability ledger), MedEinst and MedQA only.
