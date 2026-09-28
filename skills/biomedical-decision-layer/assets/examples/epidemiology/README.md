# Interpreting epidemiological text

**Synthetic demonstration. No inference or clinical interpretation was performed.**

A researcher wants to separate historical renal disease, the experiencer of diabetes and pneumonia status at discharge. The excerpts are wholly synthetic. They extend the workbook temporal and experiencer examples.

## Inputs and mapping

Read the [source dossier](source-dossier.md), then the [source-to-field map](data-map.csv). All fields come from that fictional source. The [selected questions](questions.json) preserve the original options and version. The [assembled contexts](contexts.json) show a sufficient case for each selected question and additional missing/conflicting variants.

| Selected ID | Why it is relevant |
|---|---|
| EC01 | Does the passage place this condition before the current episode or describe new onset during it? |
| EC04 | Whose condition or exposure is described by this passage? |
| EC05 | What status does the passage explicitly assign to the condition at the relevant time? |

## Three preparation outcomes

- **Sufficient for the bounded question:** the source explicitly provides the decision-relevant facts. This does not establish overall analytical validity or biomedical truth. Token length remains unverified.
- **Missing evidence:** null fields and the restricted source excerpt preserve the gap. Retrieve material or review; do not infer a negative result or fill from general knowledge.
- **Conflicting evidence:** both records remain visible under distinct evidence IDs. Review their comparability and provenance; do not relabel conflict as missingness or average it away.

## Future routing and handoff

The supplied rule outputs can bypass Jev/Laya for the sufficient cases after checking their definitions and source spans. Preserve the rule provenance separately from not-run model records. EC01 concerns renal timing; EC04 concerns the experiencer of diabetes; EC05 concerns pneumonia at discharge. Do not combine them into a diagnosis. Missing history requires retrieval or an unresolved/insufficient distinction reviewed by a curator. The simultaneous discharge contradiction goes to record review; repeated notes do not outvote it.

[Result records](results-not-run.json) are all `not_run`, with null model answers and probabilities. Routing above is conditional planning, not a model result or reference label. No label has been inserted into the context as an upstream verdict.

Implementation still needs expert-reviewed category boundaries, pinned backend/tokenizer, token measurement, source/dependence checks, grouped evaluation data and a validated routing policy. Cache each source extraction by its study/experiment/version; do not extract it again for every candidate. Keep the three scenarios separate and never mix their facts into a single source record.
