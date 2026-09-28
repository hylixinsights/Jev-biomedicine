# Prioritizing variants for research

**Synthetic demonstration. No inference or clinical interpretation was performed.**

A researcher has an already-annotated variant and wants to prepare mechanism and assay relevance questions. The demonstration expands the workbook GV01, GV03 and GV04 patterns. No pathogenicity classification is made.

## Inputs and mapping

Read the [source dossier](source-dossier.md), then the [source-to-field map](data-map.csv). All fields come from that fictional source. The [selected questions](questions.json) preserve the original options and version. The [assembled contexts](contexts.json) show a sufficient case for each selected question and additional missing/conflicting variants.

| Selected ID | Why it is relevant |
|---|---|
| GV01 | Is the variant's proposed molecular effect compatible with the mechanism described for this gene–disease relationship? |
| GV03 | Does the functional experiment test the mechanism relevant to this variant–disease relationship? |
| GV04 | Does the assay system represent the transcript or isoform implicated by the proposed variant mechanism? |

## Three preparation outcomes

- **Sufficient for the bounded question:** the source explicitly provides the decision-relevant facts. This does not establish overall analytical validity or biomedical truth. Token length remains unverified.
- **Missing evidence:** null fields and the restricted source excerpt preserve the gap. Retrieve material or review; do not infer a negative result or fill from general knowledge.
- **Conflicting evidence:** both records remain visible under distinct evidence IDs. Review their comparability and provenance; do not relabel conflict as missingness or average it away.

## Future routing and handoff

Evaluate GV01 compatibility separately from GV03 assay relevance and GV04 transcript representation. A predicted truncation does not establish measured loss of function, disease causation or pathogenicity. An opposing-mechanism label would flag the candidate for research review rather than automatic exclusion. A generic-disruption assay does not settle the channel mechanism. Missing transcript or mechanism evidence blocks automatic evaluation. Conflicting X1/X3 mechanism records require expert adjudication before a single disease mechanism is used.

[Result records](results-not-run.json) are all `not_run`, with null model answers and probabilities. Routing above is conditional planning, not a model result or reference label. No label has been inserted into the context as an upstream verdict.

Implementation still needs expert-reviewed category boundaries, pinned backend/tokenizer, token measurement, source/dependence checks, grouped evaluation data and a validated routing policy. Cache each source extraction by its study/experiment/version; do not extract it again for every candidate. Keep the three scenarios separate and never mix their facts into a single source record.
