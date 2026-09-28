# Validation plan

No scientific evaluation has been run. File checks establish packaging and transcription integrity only. Synthetic preparation examples do not measure biomedical validity or model accuracy.

Define expert-reviewed answer boundaries before collecting labels. Compare rules and specialist outputs with any decision-model candidate on the same source evidence. Measure retrieval/extraction failures separately from semantic decision failures. Keep a useful-candidate retention audit so uncertainty does not silently remove promising biology.

Split at biological and source dependence units before making contexts. Fit thresholds and calibration on development data and preserve an untouched final test set. A single confidence threshold across all questions is not justified. Evaluate abstention and conflicts separately from negative evidence.

The following 16 proposals are preserved from the workbook's Validation sheet. They remain **proposed — not run**.

## V01: Scope and label definitions

Define the intended use, unit, options, mixed/conflicting precedence and missing-evidence policy with domain experts.

**Measure:** Inter-rater agreement; disagreements requiring revised definitions.

**Prevent:** No downstream model result available to labelers.

**Scope:** All domains

## V02: Rule-first baseline

Compare the proposed layer against existing rules, ontology matching, specialist software and a compact supervised classifier where appropriate.

**Measure:** Per-class precision/recall; throughput; end-to-end cost.

**Prevent:** Remove questions already solved by structured fields from the Jev/Laya workload.

**Scope:** All domains

## V03: Context preparation ablation

Compare original retrieved spans with cached LLM-extracted facts, holding the question and candidate set fixed.

**Measure:** Extraction errors; missing decisive facts; decision accuracy; preparation cost.

**Prevent:** Do not give Jev/Laya an upstream LLM answer to the same question.

**Scope:** All domains

## V04: Human reference labels

Use independently labeled real task cases, with expert adjudication; retain difficult and rare categories.

**Measure:** Agreement; class support; adjudicated error types.

**Prevent:** LLM-generated labels may assist training but are not an independent gold standard.

**Scope:** All domains

## V05: Leakage-resistant splitting

Split at the biological/source dependence unit: patient, family, donor, slide, study or locus as applicable.

**Measure:** Performance on unseen sources, cohorts and institutions.

**Prevent:** Keep reused excerpts, candidate-panel relatives and duplicate experiments in one split.

**Scope:** All domains

## V06: Model-specific training and calibration

Evaluate pinned Jev and Laya versions separately. Fit any task adaptation and calibration only on development/calibration data.

**Measure:** Macro-F1; class recall; Brier score; reliability plots; calibration by question/domain.

**Prevent:** Keep a final held-out test set. Laya's model card warns about zero-shot limits and overconfidence; no biomedical transfer is assumed.

**Scope:** S01–S04; all domains

## V07: Selective prediction

Choose routing thresholds from the accepted error rate for each task, not an arbitrary global confidence cut-off.

**Measure:** Risk–coverage curve; retained-candidate recall; review workload.

**Prevent:** Always route missing context and out-of-domain inputs independently of reported confidence.

**Scope:** All domains

## V08: Retrieval and extraction quality

Audit retrieved methods/results, entity identity, supplementary data, negative results and source access.

**Measure:** Recall of decisive evidence; source-link accuracy; unsupported extraction rate.

**Prevent:** Do not score missing retrieved evidence as a biological negative or proof of novelty.

**Scope:** Evidence-based questions

## V09: Counterfactual context tests

Change one relevant fact: specimen, transcript, comparator, temporal anchor, marker condition or assay format.

**Measure:** Expected label-change rate and irrelevant-change stability.

**Prevent:** Synthetic tests check sensitivity, not real-world biomedical performance.

**Scope:** All domains

## V10: Question and option stability

Vary option order and equivalent wording; test mixed, contradictory and insufficient cases.

**Measure:** Label/probability stability and systematic option bias.

**Prevent:** Short option sets must still be distinguishable; inspect tokenized input, not just character count.

**Scope:** All domains

## V11: Pipeline-level comparison

Compare scripts → LLM/expert with scripts → decision layer → LLM/expert using the same retrieved evidence and final review budget.

**Measure:** Relevant candidates retained; time to review; end-to-end errors and costs.

**Prevent:** An accurate local classifier can still harm the workflow by filtering out useful candidates.

**Scope:** All domains

## V12: Candidate-loss audit

Sample retained, lowered-priority, bypassed and abstained cases; deliberately inspect rare/poorly annotated candidates.

**Measure:** False-negative candidate rate; rare-class recall; under-annotation bias.

**Prevent:** Preserve a contradiction and discovery queue. No blanket exclusion for insufficient evidence.

**Scope:** Discovery/prioritization

## V13: Image and spectrum dependency audit

Evaluate descriptor generation and semantic decision separately, then together against image/spectrum-grounded expert labels.

**Measure:** Upstream-versus-decision error attribution; rare-lesion or isomer loss.

**Prevent:** Never claim direct Jev/Laya image/spectral understanding from results on prepared text.

**Scope:** SC / ML / FC / HI

## V14: Cost accounting

Measure preprocessing, retrieval, extraction, decision calls, downstream LLM calls, review and cache reuse under the actual workload.

**Measure:** Total cost per retained useful candidate; amortized extraction cost; latency percentiles.

**Prevent:** Scripts and self-hosted models have compute/storage/maintenance costs; there is no zero-cost assumption.

**Scope:** All domains

## V15: Scientific validation beyond text labels

Validate the biological target: held-out protein-panel performance, perturbation, analytical identity, gate membership or pathology interpretation.

**Measure:** Task-specific experimental/clinical research endpoint.

**Prevent:** Semantic-label agreement alone is not a discovery, biomarker validation or clinical validation.

**Scope:** All domains

## V16: Governance and deployment

Version sources, definitions, checkpoints and rules; minimize personal data; obtain approvals appropriate to data and intended use.

**Measure:** Traceability; access controls; audit completeness; dataset/model drift.

**Prevent:** Use research routing only until the applicable clinical and institutional validation requirements are met.

**Scope:** All domains

## Preparation acceptance checks

A researcher should be able to supply a plain-language research question and a few source columns. The assistant must select relevant IDs, map actual fields, state gaps, propose a traceable context and identify rule bypasses. It must not manufacture an absent transcript, infer a matrix from a brochure heading, treat missing history as new onset, or populate a model result before execution.

Use the three [worked examples](../assets/examples/index.md) for manual scenario checks. Actual model testing, blinded human labeling and clinical/experimental validation are still future work. This repository records no measured model performance, speedup, calibration gain or cost reduction.
