# Selecting biomarker evidence

**Synthetic demonstration. No inference or clinical interpretation was performed.**

A researcher wants to add candidate C to a fixed A/B panel for severe-versus-mild inflammatory disease and evaluate assay evidence for EDTA plasma. This synthetic extension develops the workbook BM01–BM03 examples. The package separates biological complementarity, matrix validation and cohort applicability from predictive performance.

## Inputs and mapping

Read the [source dossier](source-dossier.md), then the [source-to-field map](data-map.csv). All fields come from that fictional source. The [selected questions](questions.json) preserve the original options and version. The [assembled contexts](contexts.json) show a sufficient case for each selected question and additional missing/conflicting variants.

| Selected ID | Why it is relevant |
|---|---|
| BM01 | Does the candidate add a disease-relevant biological process not already represented by the panel? |
| BM02 | Does the cited assay-validation experiment support use in the proposed specimen matrix? |
| BM03 | Does the study comparison address the intended clinical distinction? |

## Three preparation outcomes

- **Sufficient for the bounded question:** the source explicitly provides the decision-relevant facts. This does not establish overall analytical validity or biomedical truth. Token length remains unverified.
- **Missing evidence:** null fields and the restricted source excerpt preserve the gap. Retrieve material or review; do not infer a negative result or fill from general knowledge.
- **Conflicting evidence:** both records remain visible under distinct evidence IDs. Review their comparability and provenance; do not relabel conflict as missingness or average it away.

## Future routing and handoff

Keep BM01 biological roles, BM02 matrix evidence and BM03 cohort applicability as separate tags. A future complementary-process label retains C for panel testing; it does not establish incremental AUC. Matching matrix and cohort labels permit further research review, not clinical deployment. Missing matrix evidence goes to source retrieval. The X2/X4 conflict goes to an assay expert with both experiments attached; C is not automatically discarded.

[Result records](results-not-run.json) are all `not_run`, with null model answers and probabilities. Routing above is conditional planning, not a model result or reference label. No label has been inserted into the context as an upstream verdict.

Implementation still needs expert-reviewed category boundaries, pinned backend/tokenizer, token measurement, source/dependence checks, grouped evaluation data and a validated routing policy. Cache each source extraction by its study/experiment/version; do not extract it again for every candidate. Keep the three scenarios separate and never mix their facts into a single source record.
