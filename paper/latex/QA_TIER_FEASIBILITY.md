# Can the paper rest on medical MCQ / open-ended QA without clinicians?

Short answer: yes for evaluation and for part of the training, with one
condition. Every label must already exist in a released benchmark or follow
mechanically from one. v4 does this; calculators stay as the only exact tier.

## What comparable work evaluates on (checked against the sources)

| Work | Benchmarks |
|---|---|
| Med-PRM (EMNLP 2025) | five medical QA benchmarks plus two open-ended diagnostic tasks; MedQA is the headline |
| MedPRMBench (2026) | chains from seven medical QA sources, 14 injected error types, PRMScore; downstream MedQA |
| MedEinst (ACL 2026) | 5,383 control/trap pairs built on DDXPlus; Bias Trap Rate; 10,689 reference pairs for training |
| NLI4CT-P (SemEval-2024 Task 2) | expert-written statements about trial reports with label-preserving and label-inverting interventions; faithfulness, consistency |
| MedCalc-Bench / MedCalc-Eval | calculators; used by calculation-specific work, not by medical PRM papers |

None of the medical reward papers uses calculators as its main benchmark. None
of them changes the case under a fixed claim either, which is the gap.

## Option-by-option verdict

| Option | Feasible without clinicians | What it can establish | Verdict in v4 |
|---|---|---|---|
| Unchanged MCQ (MedQA, MedXpertQA-Text) | yes | answer selection against the key | downstream reranking; plus two label-free controls (vignette withheld, vignette swapped) |
| Exam key pairs (two questions whose keyed answers are each other's distractors) | yes, mined; labels follow from the keys | whether a verdict on an answer depends on the vignette | new clinical-tier test and training source; yield must be measured |
| MedEinst | yes, released | sensitivity to changed discriminating findings | primary clinical endpoint; labels were filtered by the benchmark's judges, so "agreement with MedEinst", not exact |
| NLI4CT-P | yes, released | faithfulness and consistency on clinical-trial evidence | zero-shot test; interventions edit the statement, not the report |
| MedPRMBench | yes, released | step-error detection; vignette dependence when the stem is withheld | PRMScore and the label-free dependence test |
| MedMCQA | yes | recall questions, mostly no vignette | dropped: little to check against a case |
| PubMedQA | yes | yes/no/maybe from an abstract | dropped: a swapped abstract has no defensible label |
| HealthBench | yes, with its model grader | open-ended response quality under physician-written rubrics | appendix only, exploratory: grader is a model, most rubric items are completeness and communication, the ledger is not built for conversations |
| LLM-edited vignettes with LLM-approved labels | yes | nothing as test ground truth | not used for testing; not used for training either in v4 |
| Calculators (executable rules) | yes | exact step-level labels, null and missing twins, the factorial | kept as the exact tier and as the source of symbolic supervision |

## Why calculators stay

* Only executable rules give exact labels for every step, and null and
  missing-value twins. The decomposition, the factorial and the claim about
  symbolic supervision need them.
* MedEinst, the natural replacement, was itself built by editing cases and
  filtering with judges. Using it as the sole basis would put the paper's
  central test on labels of that grade.
* What changes is their role: identification and training signal, not the
  headline evaluation.

## Risks specific to the clinical tier

1. Key-pair yield and difficulty are unknown until mined; near pairs may be few.
2. Transfer from rule twins to MedEinst may be small. The analysis plan
   states that a null result is reported as a negative answer outside rules.
3. Training on MedEinst reference pairs makes the MedEinst test
   in-distribution; the disease-held-out split is the stricter number.
4. Ledger targets outside rules are weak supervision (text diff; selection by
   block consistency).
5. Closed judges will likely lead on MedXpertQA-Text and key pairs.

## Minimal set if time is short (deadline order)

1. Audit on rule twins and MedEinst; vignette-withheld test on MedPRMBench.
2. Rule factorial (12 cells).
3. Transfer: rules only vs rules plus QA blocks on MedEinst and key pairs.
4. Reranking on MedQA with the swapped-vignette control.
Everything else (NLI4CT-P, MedXpertQA-Text, HealthBench, GRPO, probes) can be
cut without losing the argument.
