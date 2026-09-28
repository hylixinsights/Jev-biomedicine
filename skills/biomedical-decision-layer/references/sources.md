# Source register

Technical sources below were inspected on **2026-09-28**. They support interface and packaging descriptions, not the biomedical performance of the catalogue. No inference endpoint was called and no weights were downloaded.

| Source | Inspected revision | Scope |
|---|---|---|
| [TypeSafe introduction](https://docs.typesafe.ai/introduction) | Live documentation; no immutable revision supplied | Atomic decisions and external composition |
| [TypeSafe API reference](https://docs.typesafe.ai/api) | Live documentation | Endpoint, authentication and request/response fields |
| [TypeSafe Choice](https://docs.typesafe.ai/primitives/choice) | Live documentation | Finite categories and criteria mapping |
| [TypeSafe confidence](https://docs.typesafe.ai/confidence) | Live documentation | Provider confidence semantics |
| [receptron Laya README](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/README.md) | `6478649e723122ca24bbf5fb69ed1010023c9750` | Node/ONNX integration requirements |
| [Laya package metadata](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/package.json) | `@receptron/laya` 0.1.2 | Package identity and Node requirement |
| [Laya input packing](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/src/sequence.ts) | Same pinned commit | Header, option and state truncation behavior; not runtime-tested |
| [Laya model card](https://huggingface.co/convaiinnovations/laya) | Live `main` card; no checkpoint revision pinned by this package | Checkpoint family, limitations and linked training resources |
| [Laya training repository](https://github.com/NandhaKishorM/laya/tree/9d955671415fc19f069b9cc998928075c1f255ec) | HEAD `9d955671415fc19f069b9cc998928075c1f255ec` recorded; training recipe not inspected end to end | Future training entry point, not a verified biomedical recipe |
| [Agent Skills specification](https://agentskills.io/specification) | Live specification | Required manifest and optional supporting resources |
| [OpenAI Build skills](https://learn.chatgpt.com/docs/build-skills) | Live documentation; redirected from developers.openai.com/codex/skills | Local Codex discovery and invocation |
| [OpenAI Plugins](https://learn.chatgpt.com/docs/plugins) | Live documentation | Connecting an available GitHub plugin for repository retrieval |

The consulted TypeSafe pages did not establish hosted fine-tuning support. The Laya README and packer differ in their descriptions of overlong options; the future adapter must resolve actual caller behavior. The source date is an inspection date, not a guarantee of future compatibility.

## Workbook source register

The following entries are inherited verbatim from the original workbook. Their access dates are **workbook-reported**. Except for the technical sources explicitly listed above, these pages were not independently re-audited for this release. They are background references for upstream methods and evidence concepts, not training labels or validation of Jev/Laya.

### S01: TypeSafe — Introduction

[Official documentation](https://docs.typesafe.ai/introduction)

**Workbook scope:** Typed questions over a shared state; separate atomic decisions and combine them in code.

**Limitation:** Describes the interface/design pattern, not biomedical validation.

Workbook-reported access: 2026-09-28.

### S02: TypeSafe — Choice

[Official documentation](https://docs.typesafe.ai/primitives/choice)

**Workbook scope:** Choice questions, criteria and structured choice outputs.

**Limitation:** Check the current API schema when implementing an adapter.

Workbook-reported access: 2026-09-28.

### S03: TypeSafe — Confidence

[Official documentation](https://docs.typesafe.ai/confidence)

**Workbook scope:** Provider-defined confidence and decision routing.

**Limitation:** Provider confidence is not automatically a calibrated probability of biomedical correctness.

Workbook-reported access: 2026-09-28.

### S04: Laya — model card

[Developer model card](https://huggingface.co/convaiinnovations/laya)

**Workbook scope:** Text/JSON state, typed decisions, checkpoint/token-budget differences; warns about zero-shot performance and calibration.

**Limitation:** Developer-reported benchmarks are not validation of these biomedical questions. Pin checkpoint and runtime versions.

Workbook-reported access: 2026-09-28.

### S05: DESeq2

[Official Bioconductor documentation](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)

**Workbook scope:** Count-based differential-expression analysis as an upstream statistical task.

**Limitation:** Differential expression alone does not establish causality, protein abundance or biomarker performance.

Workbook-reported access: 2026-09-28.

### S06: Reactome — Content Service

[Official database documentation](https://reactome.org/dev/content-service)

**Workbook scope:** Programmatic retrieval of pathway records and biological annotations.

**Limitation:** Annotations are evidence inputs; absence of an annotation is not evidence of irrelevance.

Workbook-reported access: 2026-09-28.

### S07: Ensembl VEP — release 116 archive

[Official documentation](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)

**Workbook scope:** Variant consequences, reference annotations and population frequencies can be computed/retrieved upstream.

**Limitation:** Predicted molecular consequence is not equivalent to disease causation.

Workbook-reported access: 2026-09-28.

### S08: ClinGen — Variant Classification Guidance

[Official guidance portal](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)

**Workbook scope:** Current portal for aggregated, mechanism- and evidence-specific variant-classification recommendations.

**Limitation:** Research triage does not replace professional variant classification or the applicable evidence framework.

Workbook-reported access: 2026-09-28.

### S09: CellTypist documentation

[Official software documentation](https://celltypist.readthedocs.io/en/latest/)

**Workbook scope:** Automated cell-type annotation and prediction outputs as upstream inputs.

**Limitation:** Do not replace a trained classifier with marker-name intuition; assess discordant cases.

Workbook-reported access: 2026-09-28.

### S10: ENCODE SCREEN

[Official resource](https://screen.encodeproject.org/)

**Workbook scope:** Registry of candidate cis-regulatory elements derived from ENCODE data.

**Limitation:** Candidate-element annotation alone does not prove a target gene or regulatory mechanism.

Workbook-reported access: 2026-09-28.

### S11: SIRIUS documentation

[Official software documentation](https://v6.docs.sirius-ms.io/)

**Workbook scope:** Specialist mass-spectrometry tools produce molecular-formula, structure-candidate and chemical-class annotations.

**Limitation:** Contextual plausibility must not override analytical evidence or identify unresolved isomers.

Workbook-reported access: 2026-09-28.

### S12: HUMAnN — laboratory documentation

[Official software documentation](https://huttenhower.sph.harvard.edu/humann/)

**Workbook scope:** Profiling microbial molecular functions and metabolic pathways from sequencing data.

**Limitation:** Functional potential and measured metabolite production are different evidence types.

Workbook-reported access: 2026-09-28.

### S13: Harkema et al. — ConText (2009)

[Primary research article](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

**Workbook scope:** Rule-based negation, experiencer and temporality extraction; baseline for short clinical-text processing.

**Limitation:** Use semantic triage only for unresolved contextual cases, not as a replacement for simple reliable rules.

Workbook-reported access: 2026-09-28.

### S14: openCyto vignette

[Official Bioconductor documentation](https://bioconductor.org/packages/release/bioc/vignettes/openCyto/inst/doc/openCytoVignette.html)

**Workbook scope:** Template-based, data-driven flow-cytometry gating.

**Limitation:** Gate geometry, compensation and numerical quality checks remain upstream.

Workbook-reported access: 2026-09-28.

### S15: QuPath — About

[Official software documentation](https://qupath.readthedocs.io/en/stable/docs/intro/about.html)

**Workbook scope:** Digital-pathology image analysis provides regions, objects and measurements.

**Limitation:** The proposed decision layer consumes descriptors; direct slide-image input is not assumed.

Workbook-reported access: 2026-09-28.

### S16: PMC — Open Access Web Service

[Official API documentation](https://pmc.ncbi.nlm.nih.gov/tools/oa-service/)

**Workbook scope:** Programmatic discovery of reusable open-access article resources.

**Limitation:** Availability and permitted reuse depend on the article/license. Retrieval coverage must be recorded.

Workbook-reported access: 2026-09-28.

### S17: Uhlen et al. — A proposal for validation of antibodies (2016)

[Original expert-group proposal](https://pmc.ncbi.nlm.nih.gov/articles/PMC10335836/)

**Workbook scope:** Antibody validation is application- and context-specific; identity and experimental validation should be documented.

**Limitation:** Supports examining validation scope, not ranking a particular antibody or validating a Jev/Laya task.

Workbook-reported access: 2026-09-28.

### S18: MSstats

[Official Bioconductor documentation](https://bioconductor.org/packages/release/bioc/html/MSstats.html)

**Workbook scope:** Statistical analysis of mass-spectrometry-based protein quantification as an upstream task.

**Limitation:** Quantification and statistical testing remain in specialist software, not the semantic decision model.

Workbook-reported access: 2026-09-28.
