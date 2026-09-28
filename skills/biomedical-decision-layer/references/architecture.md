# Architecture and responsibilities

The scientific workflow is data → specialist processing → traceable context → bounded decisions → application rules → deeper LLM/expert analysis. This repository prepares the middle interfaces without implementing execution.

| Component | Responsibility | Example |
|---|---|---|
| Researcher | Fix intended use, unit and comparison | Pre-treatment severe versus mild disease |
| Scripts and specialist tools | Compute statistics, QC, annotation and descriptors | Differential expression, variant consequences, gate summaries |
| Retrieval and optional LLM extraction | Recover factual spans with locations | Tested specimen, construct, endpoint and limitations |
| Decision model, in a future pilot | Answer one bounded semantic question | Whether the tested assay matrix matches the planned matrix |
| Application rules | Combine separate answers with measured results | Preserve promising candidates while requesting matrix validation |
| Qualified expert | Resolve conflicts and assess the scientific claim | Design analytical or experimental validation |

[TypeSafe's introduction](https://docs.typesafe.ai/introduction) describes atomic decisions and external composition. Our biomedical use is a proposed application of that pattern. A shared input can support independent questions; questions do not acquire knowledge from one another's answers. If one decision truly depends on another, specify separate stages.

Extract once per source version and relevant biological context. A gene–tissue–state fact can be reused across panels; an assay's matrix validation can be reused across candidates; study eligibility can be reused across all genes in a study. Cache source spans, extraction method, extractor version and reviewer status. Invalidate when the source, relevant context or extraction method changes. Record the selected-panel snapshot separately so changing the panel does not rewrite source facts.

Do not ask the model to recompute correlations, AUC, statistical tests, genomic annotation, spectral matching, image segmentation or gates. Use reliable structured rules whenever fields already settle the question. Runtime confidence cannot repair an invalid statistical result or an incorrectly extracted source.
