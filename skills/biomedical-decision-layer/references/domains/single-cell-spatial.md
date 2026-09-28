# Single-cell & spatial

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[SC01](#sc01) | [SC02](#sc02) | [SC03](#sc03) | [SC04](#sc04) | [SC05](#sc05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="sc01"></a>

## SC01

**Question:** Does the supplied marker evidence primarily support lineage identity, a shared activation state, or a mixed-lineage signal?

**Decision unit:** One ambiguous cell cluster × proposed annotation

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Lineage identity | Markers predominantly identify a lineage. |
| 2 | Shared activation state | Markers predominantly describe an activation program shared across lineages. |
| 3 | Mixed-lineage signal | Markers indicate more than one lineage and require mixture or technical review. |
| 4 | Lineage plus activation state | Both lineage identity and activation-state evidence are present. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Cluster marker summaries + reference marker-function descriptions

**Minimum context:** Top positive/negative markers; detection fractions; competing labels; tissue and treatment; reference definitions of lineage and state.

| Required field | Proposed supplier |
|---|---|
| `cluster_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `top_markers` | Existing statistical or specialist output; record tool, version, units and QC |
| `negative_markers` | Existing statistical or specialist output; record tool, version, units and QC |
| `classifier_labels` | Existing statistical or specialist output; record tool, version, units and QC |
| `tissue` | Provided study metadata or source record; researcher confirms identity and meaning |
| `state_reference` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Run QC, doublet detection and a trained annotator first; compute marker effects/fractions; retrieve descriptions only for unresolved annotations.

**Optional cached LLM extraction:** Optionally curate marker-function descriptions once per reference/source, not separately for every cell.

**Reuse key:** cluster × reference model version; marker library reusable across cells

**Semantic work remaining:** Separates a state label from a lineage label when biological names overlap.

**Bypass Jev/Laya:** If a validated classifier/rule already resolves the annotation, bypass. Do not send the full expression matrix.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** SC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine identity, state and conflict tags by cluster/region; project to cells only under validated annotation rules.

**Future routing:** Lineage/state/both → retain separate identity/state labels; mixed/U → inspect doublets, ambient RNA and reference mismatch.

**Expert follow-up:** Inspect markers, embeddings, images where relevant and reference mismatch; a cell/tissue expert adjudicates.

**Interpretation limit:** A mixed marker pattern is not proof of a novel lineage, transdifferentiation or a doublet.

**Proposed validation:** Expert labels on discordant clusters and an audit sample of concordant cells; hold out donors.

### SC01 synthetic example

Input material: Count matrix, cluster metadata, classifier outputs, marker tables and reference-cell descriptions.

SYNTHETIC: A cluster retains T-lineage markers but its strongest differential markers are a response program shared by several lineages in the reference. The classifier labels it as T cells.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "SC01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-sc01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "SC01-E1",
      "source_id": "workbook-synthetic-SC01",
      "source_location": "Context_design!G22",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A cluster retains T-lineage markers but its strongest differential markers are a response program shared by several lineages in the reference. The classifier labels it as T cells."
    }
  ],
  "context_fields": {
    "cluster_id": null,
    "top_markers": "strongest differential markers form a shared response program",
    "negative_markers": null,
    "classifier_labels": [
      "T cells"
    ],
    "tissue": null,
    "state_reference": "Response program shared by several lineages; T-lineage markers retained."
  },
  "missing_fields": [
    "cluster_id",
    "negative_markers",
    "tissue"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing cluster_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 22, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://celltypist.readthedocs.io/en/latest/)

<a id="sc02"></a>

## SC02

**Question:** What best explains the disagreement between the two cell labels?

**Decision unit:** One pair of discordant cell labels

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Equivalent terminology | The label definitions describe the same biological entity. |
| 2 | Different annotation granularity | One label is a finer subdivision of the other. |
| 3 | Substantive biological disagreement | Definitions imply biologically incompatible assignments. |
| 4 | Labels cannot be resolved from supplied definitions | Supplied definitions are present but do not resolve their relation. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Classifier labels + ontology definitions + marker summaries

**Minimum context:** Two labels and definitions; available parent/child relationships; marker evidence relevant to their difference.

| Required field | Proposed supplier |
|---|---|
| `label_A` | Provided study metadata or source record; researcher confirms identity and meaning |
| `label_B` | Provided study metadata or source record; researcher confirms identity and meaning |
| `label_definitions` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `known_ontology_relations` | Provided study metadata or source record; researcher confirms identity and meaning |
| `marker_summary` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Resolve exact synonyms and parent–child relations first; route only unmapped or context-dependent terms with their definitions.

**Optional cached LLM extraction:** Once per label source, extract the intended population definition when no ontology mapping exists.

**Reuse key:** label pair × ontology/reference release; reuse across every affected cell

**Semantic work remaining:** Interprets unresolved local annotation terminology rather than assuming every string mismatch is an error.

**Bypass Jev/Laya:** Use ontology graph traversal or a curated synonym map whenever it resolves the pair.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** SC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine identity, state and conflict tags by cluster/region; project to cells only under validated annotation rules.

**Future routing:** Equivalent/granularity → harmonize via code; biological disagreement/unresolved/U → expert review.

**Expert follow-up:** Inspect markers, embeddings, images where relevant and reference mismatch; a cell/tissue expert adjudicates.

**Interpretation limit:** Do not claim improved annotation accuracy from harmonizing terminology alone.

**Proposed validation:** Expert label-pair equivalence set; evaluate biological corrections separately from naming changes.

### SC02 synthetic example

Input material: Annotator outputs, Cell Ontology mappings, reference-model descriptions and marker table.

SYNTHETIC: Annotator A reports 'T cell'; annotator B reports 'activated CD8 T cell'. The reference description states that B is a CD8 subset within the T-cell lineage.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "SC02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-sc02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "SC02-E1",
      "source_id": "workbook-synthetic-SC02",
      "source_location": "Context_design!G23",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Annotator A reports 'T cell'; annotator B reports 'activated CD8 T cell'. The reference description states that B is a CD8 subset within the T-cell lineage."
    }
  ],
  "context_fields": {
    "label_A": "T cell",
    "label_B": "activated CD8 T cell",
    "label_definitions": "B is a CD8 subset within the T-cell lineage.",
    "known_ontology_relations": "B described as a subset of A in the supplied reference.",
    "marker_summary": null
  },
  "missing_fields": [
    "marker_summary"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing label_A from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 23, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://celltypist.readthedocs.io/en/latest/)

<a id="sc03"></a>

## SC03

**Question:** Does the region description support the tissue organization required by the research question?

**Decision unit:** One spatial neighborhood × required tissue organization

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Required organization described | Validated descriptors include the organization required by the study. |
| 2 | Relevant cells without required organization | Relevant cell types are present without the required spatial organization. |
| 3 | Different tissue compartment | Descriptors place the region in a different tissue compartment. |
| 4 | Conflicting region descriptions | Relevant descriptors disagree about region organization. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Spatial graph summaries + region descriptions + organization definition

**Minimum context:** Human-defined organization criteria; computed proximity/segregation descriptors; evidence about boundaries and interfaces.

| Required field | Proposed supplier |
|---|---|
| `required_organization` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `region_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `cell_composition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `spatial_descriptors` | Provided study metadata or source record; researcher confirms identity and meaning |
| `morphology_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `uncertainty` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute distances, densities, neighborhoods and compartment adjacency. Produce a bounded descriptor table, retaining uncertain labels.

**Optional cached LLM extraction:** No LLM needed for numerical descriptors. A vision model may supply morphology descriptions only when required, with separate validation and provenance.

**Reuse key:** region × segmentation/annotation version × research definition

**Semantic work remaining:** Matches morphology language to an explicit organization definition; cell abundance alone is insufficient.

**Bypass Jev/Laya:** If every organization criterion is a validated numerical predicate, apply it in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** SC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine identity, state and conflict tags by cluster/region; project to cells only under validated annotation rules.

**Future routing:** Required → retain region; cells-only/different → different analysis stratum; conflict/U → image review.

**Expert follow-up:** Inspect markers, embeddings, images where relevant and reference mismatch; a cell/tissue expert adjudicates.

**Interpretation limit:** This is not direct image interpretation, and inferred spatial organization is limited by upstream segmentation and descriptors.

**Proposed validation:** Pathologist labels plus image-grounded descriptor accuracy; split by patient/slide.

### SC03 synthetic example

Input material: Cell coordinates, cell types, segmentation masks, neighborhood features and image/pathologist descriptors.

SYNTHETIC: The question requires an organized lymphoid aggregate with distinct B- and T-cell zones. The region has both cell types, but the validated descriptor reports diffuse intermixing without zones.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "SC03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-sc03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "SC03-E1",
      "source_id": "workbook-synthetic-SC03",
      "source_location": "Context_design!G24",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The question requires an organized lymphoid aggregate with distinct B- and T-cell zones. The region has both cell types, but the validated descriptor reports diffuse intermixing without zones."
    }
  ],
  "context_fields": {
    "required_organization": "organized lymphoid aggregate with distinct B- and T-cell zones",
    "region_id": null,
    "cell_composition": [
      "B cells",
      "T cells"
    ],
    "spatial_descriptors": "diffuse intermixing without zones",
    "morphology_excerpt": "Validated descriptor reports no distinct zones.",
    "uncertainty": null
  },
  "missing_fields": [
    "region_id",
    "uncertainty"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing required_organization from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 24, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://celltypist.readthedocs.io/en/latest/)

<a id="sc04"></a>

## SC04

**Question:** Does the supplied evidence support a processing-associated explanation for the proposed cell state?

**Decision unit:** One cluster/state × sample-processing evidence

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Direct processing experiment supports it | A controlled processing experiment reproduces the proposed state. |
| 2 | Indirect association only | State and processing co-occur without a direct processing test. |
| 3 | Evidence against processing explanation | A relevant processing comparison provides evidence against that explanation. |
| 4 | Conflicting processing evidence | Comparable processing experiments give opposing evidence. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** State signatures + dissociation metadata + control-experiment text

**Minimum context:** Candidate state definition; preparation conditions; immediate/fixed or alternative-processing controls; measured state response.

| Required field | Proposed supplier |
|---|---|
| `state_signature` | Provided study metadata or source record; researcher confirms identity and meaning |
| `processing_protocol` | Provided study metadata or source record; researcher confirms identity and meaning |
| `control_comparison` | Provided study metadata or source record; researcher confirms identity and meaning |
| `computed_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute stress/state scores and batch associations; compare available controls; retrieve the processing-intervention result.

**Optional cached LLM extraction:** Once per protocol/study, extract processing conditions and the response measured, without declaring the cluster artifactual.

**Reuse key:** processing protocol × state × reference experiment

**Semantic work remaining:** Relates a technical manipulation to the biological-state description when protocol labels are not standardized.

**Bypass Jev/Laya:** Use established QC/state rules when fully applicable; numerical association testing remains upstream.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** SC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine identity, state and conflict tags by cluster/region; project to cells only under validated annotation rules.

**Future routing:** Direct/indirect → processing-sensitivity review; against → retain state with caveat; conflicting/U → compare additional controls.

**Expert follow-up:** Inspect markers, embeddings, images where relevant and reference mismatch; a cell/tissue expert adjudicates.

**Interpretation limit:** A processing association does not prove every affected cell is artifactual; disease and handling may be confounded.

**Proposed validation:** Expert evidence-relation labels plus matched-processing validation.

### SC04 synthetic example

Input material: Expression summaries, processing times, protocols and matched processing-control studies.

SYNTHETIC: A state is enriched in enzymatically dissociated samples. E1 reproduces its transcript program after prolonged digestion of matched tissue; immediately preserved samples show little of the program.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "SC04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-sc04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "SC04-E1",
      "source_id": "workbook-synthetic-SC04",
      "source_location": "Context_design!G25",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A state is enriched in enzymatically dissociated samples. E1 reproduces its transcript program after prolonged digestion of matched tissue; immediately preserved samples show little of the program."
    }
  ],
  "context_fields": {
    "state_signature": "transcript program reproduced after prolonged digestion",
    "processing_protocol": "enzymatic dissociation",
    "control_comparison": "matched tissue with prolonged digestion versus immediate preservation",
    "computed_results": "Program enriched after digestion and low in immediately preserved tissue.",
    "experiment_excerpt": "E1 reproduces the program after prolonged digestion of matched tissue."
  },
  "missing_fields": [],
  "preparation_status": "synthetic_mapping_requires_review",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing state_signature from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 25, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://celltypist.readthedocs.io/en/latest/)

<a id="sc05"></a>

## SC05

**Question:** Does the cited experiment test the same sender–receiver interaction proposed for this tissue?

**Decision unit:** One predicted cell–cell interaction × experimental evidence

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Same sender–receiver context | The experiment tests the proposed sender, receiver and tissue interaction. |
| 2 | Related cell-pair context only | The experiment tests a related cell pair or setting. |
| 3 | One-sided cellular response only | Only one cellular response is tested without demonstrating the full interaction. |
| 4 | Interaction not tested | The stated interaction is not experimentally tested. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Ligand–receptor candidate + cell-pair metadata + experiment excerpt

**Minimum context:** Proposed sender/receiver and direction; expressed ligand/receptor; relevant co-culture or perturbation experiment and tissue context.

| Required field | Proposed supplier |
|---|---|
| `sender` | Provided study metadata or source record; researcher confirms identity and meaning |
| `receiver` | Provided study metadata or source record; researcher confirms identity and meaning |
| `ligand_receptor` | Provided study metadata or source record; researcher confirms identity and meaning |
| `tissue` | Provided study metadata or source record; researcher confirms identity and meaning |
| `interaction_evidence` | Provided study metadata or source record; researcher confirms identity and meaning |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute expression/proximity and database matching; retrieve experiments concerning the candidate interaction rather than all ligand mentions.

**Optional cached LLM extraction:** Extract which cells produced/responded and what was manipulated once per experiment; do not infer signaling from co-expression.

**Reuse key:** ligand–receptor × cell pair × tissue × evidence version

**Semantic work remaining:** Distinguishes a response to added ligand from a demonstrated interaction between the proposed cell types.

**Bypass Jev/Laya:** If sender, receiver and experiment type are curated, compare in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** SC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine identity, state and conflict tags by cluster/region; project to cells only under validated annotation rules.

**Future routing:** Same → retain context evidence; related/one-sided/not tested → mark validation gap; U → retrieve experimental details.

**Expert follow-up:** Inspect markers, embeddings, images where relevant and reference mismatch; a cell/tissue expert adjudicates.

**Interpretation limit:** Co-expression, proximity and an in-vitro response do not establish in-vivo communication.

**Proposed validation:** Experts label cell-pair applicability; experimental validation remains downstream.

### SC05 synthetic example

Input material: Interaction-inference output, cell annotations, spatial summaries and functional-study passages.

SYNTHETIC: A predicts ligand release from fibroblasts to macrophages in skin. E1 exposes isolated macrophages to recombinant ligand; no fibroblast-derived ligand or skin co-culture is tested.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "SC05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-sc05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "SC05-E1",
      "source_id": "workbook-synthetic-SC05",
      "source_location": "Context_design!G26",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A predicts ligand release from fibroblasts to macrophages in skin. E1 exposes isolated macrophages to recombinant ligand; no fibroblast-derived ligand or skin co-culture is tested."
    }
  ],
  "context_fields": {
    "sender": "fibroblasts",
    "receiver": "macrophages",
    "ligand_receptor": null,
    "tissue": "skin",
    "interaction_evidence": "isolated macrophages exposed to recombinant ligand",
    "experiment_excerpt": "No fibroblast-derived ligand or skin co-culture tested."
  },
  "missing_fields": [
    "ligand_receptor"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing sender from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 26, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://celltypist.readthedocs.io/en/latest/)
- [Background source 2](https://reactome.org/dev/content-service)
