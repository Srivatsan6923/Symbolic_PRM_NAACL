# Draft v10 (MedGuideX, CondMedQA, EHRNote-ChatQA, SURE-RAG and GSAR read in full; tests and wording corrected); results NOT measured

Title: "Learning When to Change: Symbolic Supervision for Selective Evidence
Sensitivity in Medical Reward Models" Main text 8 pages, then
Limitations, Ethical Considerations, references, appendices.

Build: pdflatex main && bibtex main && pdflatex main && pdflatex main && pdflatex main

Files: main.tex, custom.bib, acl.sty, acl_natbib.bst; REVIEW_RESPONSE_V6.md
(point-by-point validation of the three reviews of v5); SYMRM_INTEGRATION.md
(what was taken from the SymRM pilot and the second review, and what was not);
QA_TIER_FEASIBILITY.md
(why and how medical QA benchmarks were added, option by option);
REVIEW_RESPONSE.md (point-by-point validation of the earlier reviews).

## Placeholders

Every experimental number is written \ph{...} in main.tex (about 709 occurrences) and
prints in red; plot coordinates are placeholders too. No experiment has been
run. With \placeholderstrue the paper carries a draft banner and Appendix A
states the draft status. Setting \placeholdersfalse makes any remaining
\ph{...} a compile error and switches Appendix A's draft note for the
reproducibility statement. Model versions wrapped in \ph are assumptions.

## Order of experiments (decides what the paper may claim)

1. Rule library with applicability predicates and manifest (no MedCalc-Eval);
   triplet engine with invariant checks; test templates and cue phrases held
   out from training; development vs test generation.
2. Shortcut scorers and the audit of the 13 signals (Table 1; includes released
   ThinkPRM and GenPRM-7B checkpoints).
3. Table 3: corpus {natural, balanced, blocks, triplets} x representation
   {verdict, rationale, two-stage summary, value ledger, applicability ledger},
   rule data only.
4. Table 4, rule-only rows first (zero-shot MedEinst is a primary endpoint),
   then + step-error data, then clinical pairs.
5. Ablations of Sec. 7.4 (decision field, one-stage ledger, field
   interventions).
6. Reranking (Table 5) and appendix items last.

## To confirm before running

* Compute: the pilot ran on an 8 GB laptop GPU; the plan assumes much more.
  The backbone is the macro \bb in main.tex.
* Rule sources: MedCalc-Eval's repository contains only a licence file, so the
  library no longer depends on it. The count of scores you implement yourself
  (placeholder 90) sets the size of the real-rule part.
* Share of criteria whose applicability predicate counts past or family
  findings (placeholder 23%).
* MedEinst release: structured evidence per case (for ledger targets) and both
  diagnoses per pair.
* Key-pair yield after one-to-one matching and exclusions (MedQA, CareQA).
* NLI4CT-P (SemEval-2024 Task 2) files and scorer; MedPRMBench release and its
  error taxonomy (which types depend on the case).
* FoVer training data; ThinkPRM and GenPRM-7B checkpoints; a sandbox for
  executing verifier-written code (GenPRM-style row).
* CondMedQA: the paper defines a general answer for every question; confirm
  the release contains it (Appendix D test).
* EHRNote-ChatQA needs PhysioNet credentialed access (MIMIC-IV) and its data
  may not be sent to external APIs. If access does not arrive in time, delete
  the paragraph "Claim-side applicability on real notes" in Appendix D and
  restore "untested" in the Limitations heading and the abstract's last
  sentence.
* Constraint rules are compiled from public recommendations (recommendation,
  decision tree, program) after MedGuideX; its own functions are not released.
* A ConText-style trigger tagger (e.g. medspaCy) for the lexicon baseline.
* Optional, credentialed: i2b2 2010 assertions would let you test the reader
  alone on real notes; not part of the plan.
* Which API judges return token log-probabilities.
* Hou et al. 85.6% / 69.8% statement: exact conditioning.
* The CareQA / MedQA / ReMedQA overlap statements come from your audit page.

## Bibliography entries marked VERIFY in custom.bib

Hou et al. (2608.03028), Zang et al. (2512.00920), CareQA (Arias-Duart et al.), EVPV (2603.16253),
Ocampo Osorio et al. (2609.10728), ThinkPRM, R-Align, MamaBench, ClinPivot,
Kapetanovic et al., RRM, Crome, DynaGuard, Med-PRM, MedS3, Meditron3, mARC,
Gorenshtein et al. Unused entries can be deleted.

## Before submission (ARR)

1. All placeholders replaced; abstract, findings, Section 7 and conclusion
   rewritten from measured values; \placeholdersfalse; clean build.
2. Main text within 8 pages; Limitations present.
3. Bibliography verified; anonymity checked.
4. Responsible NLP checklist completed.
