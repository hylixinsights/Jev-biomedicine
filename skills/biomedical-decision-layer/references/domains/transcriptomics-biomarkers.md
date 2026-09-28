# Transcriptomics — biomarkers

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[BM01](#bm01) | [BM02](#bm02) | [BM03](#bm03) | [BM04](#bm04) | [BM05](#bm05) | [BM06](#bm06)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="bm01"></a>

## BM01

**Question:** Does the candidate add a disease-relevant biological process not already represented by the panel?

**Decision unit:** One gene added to a fixed 2–5-gene panel

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Complementary process | Adds a supported process relevant to the intended use and absent from the fixed panel roles. |
| 2 | Same underlying process | Represents a process already captured by the fixed panel. |
| 3 | Process outside the intended use | Supported role falls outside the specified biological use. |
| 4 | Mixed or context-dependent roles | Relevant roles differ across contexts or include both represented and additional processes. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Expression summary + pathway annotations + experimental text

**Minimum context:** Intended distinction; panel members and known roles; candidate function excerpts in the relevant tissue/state.

| Required field | Proposed supplier |
|---|---|
| `intended_use` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `panel_members` | Provided study metadata or source record; researcher confirms identity and meaning |
| `panel_roles` | Provided study metadata or source record; researcher confirms identity and meaning |
| `candidate_role_excerpts` | Source retrieval; optional cached factual extraction; human source check |
| `tissue` | Provided study metadata or source record; researcher confirms identity and meaning |
| `source_ids` | Local provenance index created during source inventory |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Filter low expression; run differential expression and nested panel evaluation; retrieve role annotations and short source passages. Keep statistical performance separate.

**Optional cached LLM extraction:** Once per gene/process/source, extract the tested function, cell type and experimental condition with exact source spans; do not label complementarity.

**Reuse key:** gene × tissue × state × source version; reuse across candidate panels

**Semantic work remaining:** Matches differently worded functional descriptions to the clinical question; pathway overlap alone may not resolve the biological role.

**Bypass Jev/Laya:** Use code when curated process labels already determine complementarity. Do not ask the model to calculate correlations or incremental AUC.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Complementary → retain for panel testing; same process → mark redundancy, not automatic exclusion; outside use → lower semantic priority; mixed/U → review.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** Biological complementarity does not imply extra predictive value. Do not eliminate statistically useful correlated genes by this answer alone.

**Proposed validation:** Blinded expert labels on candidate–panel pairs; separately test incremental performance with patient/cohort-held-out data.

### BM01 synthetic example

Input material: Count matrix, phenotype table, selected panels, pathway records and functional-study passages.

SYNTHETIC: Panel A/B tracks the same interferon-response program. In source E1, candidate C was experimentally linked to endothelial permeability in the target tissue. Intended use: severe versus mild disease. Statistical panel performance is not supplied as a biological role.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM01-E1",
      "source_id": "workbook-synthetic-BM01",
      "source_location": "Context_design!G6",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Panel A/B tracks the same interferon-response program. In source E1, candidate C was experimentally linked to endothelial permeability in the target tissue. Intended use: severe versus mild disease. Statistical panel performance is not supplied as a biological role."
    }
  ],
  "context_fields": {
    "intended_use": "severe versus mild disease",
    "panel_members": [
      "A",
      "B"
    ],
    "panel_roles": [
      "interferon-response program"
    ],
    "candidate_role_excerpts": [
      "E1 experimentally links C to endothelial permeability in the target tissue"
    ],
    "tissue": null,
    "source_ids": [
      "E1"
    ]
  },
  "missing_fields": [
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

**Missing-information variant:** Remove all information establishing intended_use from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 6, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="bm02"></a>

## BM02

**Question:** Does the cited assay-validation experiment support use in the proposed specimen matrix?

**Decision unit:** One assay × proposed specimen

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Direct matrix evidence | Validation directly tests the proposed specimen matrix. |
| 2 | Related matrix requiring validation | Evidence tests a related matrix whose transfer still requires validation. |
| 3 | Different matrix only | Available validation tests only a different matrix. |
| 4 | Conflicting matrix evidence | Comparable matrix-validation records give incompatible findings. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Assay methods + specimen metadata + validation results

**Minimum context:** Proposed matrix/collection method; measured material in the reference; endogenous recovery, dilution or interference results if reported.

| Required field | Proposed supplier |
|---|---|
| `assay_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_matrix` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `reference_matrix` | Provided study metadata or source record; researcher confirms identity and meaning |
| `preparation` | Provided study metadata or source record; researcher confirms identity and meaning |
| `validation_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `source_id` | Local provenance index created during source inventory |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Parse product IDs and structured matrix fields; check reported units/ranges; retrieve the methods and matrix-validation passage, not the product title alone.

**Optional cached LLM extraction:** Once per assay document, extract sample preparation and what material was actually tested. Preserve qualifications such as spiked versus endogenous.

**Reuse key:** assay × document version × matrix

**Semantic work remaining:** Resolves broad marketing or methods wording against the actual experiment; numeric range checks remain in code.

**Bypass Jev/Laya:** Use a deterministic matrix-compatibility table when both matrices and validation types are already curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Direct → retain for assay development; related/different → request target-matrix validation; conflicting/U → assay-expert review.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** A commercial kit or matching sample label is not proof of acceptable analytical performance.

**Proposed validation:** Assay scientist labels using the full validation record; include spiked-only and broadly worded product descriptions.

### BM02 synthetic example

Input material: Assay technical sheet, validation paper, laboratory SOP and planned sample type.

SYNTHETIC: Planned specimen is EDTA plasma. E1 describes recovery of added recombinant protein in buffer; E2 reports endogenous measurements in serum but no plasma experiment. The brochure's heading says 'blood samples'.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM02-E1",
      "source_id": "workbook-synthetic-BM02",
      "source_location": "Context_design!G7",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Planned specimen is EDTA plasma. E1 describes recovery of added recombinant protein in buffer; E2 reports endogenous measurements in serum but no plasma experiment. The brochure's heading says 'blood samples'."
    }
  ],
  "context_fields": {
    "assay_id": null,
    "target_matrix": "EDTA plasma",
    "reference_matrix": [
      "buffer",
      "serum"
    ],
    "preparation": null,
    "validation_excerpt": "E1: recombinant protein recovery in buffer. E2: endogenous measurements in serum; no plasma experiment.",
    "source_id": null
  },
  "missing_fields": [
    "assay_id",
    "preparation",
    "source_id"
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

**Missing-information variant:** Remove all information establishing assay_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 7, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC10335836/)

<a id="bm03"></a>

## BM03

**Question:** Does the study comparison address the intended clinical distinction?

**Decision unit:** One supporting study × intended clinical distinction

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Same clinical distinction | Case, comparator and decision time match the intended distinction. |
| 2 | Healthy controls only | Cases are compared only with healthy controls, without establishing the intended alternative condition. |
| 3 | Different clinical distinction | The study contrasts different clinical states from the intended use. |
| 4 | Mixed comparison with unresolved subgroups | Pooled comparisons do not resolve the required subgroups. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Study eligibility text + cohort/comparator metadata

**Minimum context:** Target use, time of intended testing, case definition and the reference study's actual inclusion/exclusion criteria.

| Required field | Proposed supplier |
|---|---|
| `target_contrast` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `intended_time` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `case_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `comparator_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `sampling_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Retrieve comparator names and eligibility passages; normalize explicitly stated diagnoses and sampling times; keep unresolved group definitions as text.

**Optional cached LLM extraction:** Once per study, extract cohort definitions and sampling circumstances with source spans, without judging applicability.

**Reuse key:** study × contrast × version; reuse across all genes from the study

**Semantic work remaining:** A comparator's name may hide a different diagnostic question; reading eligibility wording can resolve this.

**Bypass Jev/Laya:** If case/comparator concepts and decision time are curated, compare them in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Same → retain applicability; healthy/different → flag transfer gap; mixed/U → retrieve subgroup details.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** Do not infer latent infection status from 'healthy'. Applicability is not diagnostic accuracy.

**Proposed validation:** Human applicability labels; hold out entire studies, not genes from the same study.

### BM03 synthetic example

Input material: Protocol, participant descriptions, endpoint definitions and structured cohort metadata.

SYNTHETIC: Intended use is active versus latent TB before treatment. E1 compares treatment-naive pulmonary cases with volunteers who have no symptoms; infection testing of volunteers is not described.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM03-E1",
      "source_id": "workbook-synthetic-BM03",
      "source_location": "Context_design!G8",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Intended use is active versus latent TB before treatment. E1 compares treatment-naive pulmonary cases with volunteers who have no symptoms; infection testing of volunteers is not described."
    }
  ],
  "context_fields": {
    "target_contrast": "active versus latent TB",
    "intended_time": "before treatment",
    "case_definition": "treatment-naive pulmonary cases",
    "comparator_excerpt": "Volunteers without symptoms; infection testing not described.",
    "sampling_excerpt": "Cases sampled before treatment."
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

**Missing-information variant:** Remove all information establishing target_contrast from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 8, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="bm04"></a>

## BM04

**Question:** What evidence links this transcript-derived candidate to the proposed circulating-protein readout?

**Decision unit:** One transcript-derived candidate × protein evidence item

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Paired RNA–protein evidence in target context | Paired RNA and protein measurements address the target setting. |
| 2 | Protein evidence in another context | Protein measurements exist, but in a different setting. |
| 3 | Transcript-only evidence | The supplied evidence establishes transcript measurements only. |
| 4 | Discordant paired evidence | Paired measurements in comparable conditions disagree in the relevant direction or relationship. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Paired molecular summaries + protein-study methods

**Minimum context:** Target disease/state and specimen; whether RNA and endogenous protein were measured in the same people; computed direction/association and uncertainty.

| Required field | Proposed supplier |
|---|---|
| `target_readout` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `pairing_status` | Provided study metadata or source record; researcher confirms identity and meaning |
| `sample_context` | Provided study metadata or source record; researcher confirms identity and meaning |
| `computed_association` | Existing statistical or specialist output; record tool, version, units and QC |
| `uncertainty` | Existing statistical or specialist output; record tool, version, units and QC |
| `measurement_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Match participants/time points, calculate associations or within-person changes and uncertainty, and identify specimen types. Retrieve passages explaining what was measured.

**Optional cached LLM extraction:** Extract whether protein evidence refers to circulating endogenous protein, tissue staining or a recombinant construct when methods are unstructured.

**Reuse key:** candidate × molecular readout × study version

**Semantic work remaining:** Distinguishes what 'protein validation' actually measured; the model does not predict protein concentration from RNA.

**Bypass Jev/Laya:** Use code if pairing, molecular entity and compartment are explicit structured fields.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Target paired → keep translational evidence; other/transcript-only → mark protein-evidence gap; discordant/U → examine mechanism/assay before selection.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** RNA differential expression does not establish that an ELISA-measurable plasma signal exists.

**Proposed validation:** Labels on measurement identity and pairing; independent protein measurements needed for biological validation.

### BM04 synthetic example

Input material: Matched RNA/protein tables where available; sample manifests; study methods and results.

SYNTHETIC: Blood RNA for candidate A differs between groups. The only protein passage describes intracellular staining in cultured cells; no plasma concentration or paired participant measurement is reported.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM04-E1",
      "source_id": "workbook-synthetic-BM04",
      "source_location": "Context_design!G9",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Blood RNA for candidate A differs between groups. The only protein passage describes intracellular staining in cultured cells; no plasma concentration or paired participant measurement is reported."
    }
  ],
  "context_fields": {
    "target_readout": "circulating protein concentration",
    "pairing_status": "no paired participant measurement reported",
    "sample_context": "blood RNA; protein evidence from cultured cells",
    "computed_association": null,
    "uncertainty": null,
    "measurement_excerpt": "Only intracellular protein staining is described; no plasma concentration is reported."
  },
  "missing_fields": [
    "computed_association",
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

**Missing-information variant:** Remove all information establishing target_readout from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 9, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="bm05"></a>

## BM05

**Question:** Does the supplied evidence show that the proposed alternative explanation can generate this candidate's signal?

**Decision unit:** One candidate × documented alternative explanation

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Direct experimental support | An intervention directly tests whether the alternative explanation can generate the signal. |
| 2 | Associative support only | An observational relationship supports the alternative without an experimental test. |
| 3 | Evidence against the explanation | A relevant test provides evidence inconsistent with the named explanation. |
| 4 | Conflicting evidence | Relevant tests disagree about the named alternative. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Covariate summaries + perturbation/exposure evidence

**Minimum context:** Observed signal; one named alternative such as treatment exposure or cell-mixture change; relevant experiment and endpoint definition.

| Required field | Proposed supplier |
|---|---|
| `candidate_signal` | Provided study metadata or source record; researcher confirms identity and meaning |
| `alternative_explanation` | Provided study metadata or source record; researcher confirms identity and meaning |
| `adjusted_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `experimental_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `setting` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Fit planned adjusted/sensitivity models; report changes in the effect estimate. Retrieve a bounded evidence item for the named alternative.

**Optional cached LLM extraction:** Extract intervention, timing and measured readout once per experiment; do not infer which explanation is true in the cohort.

**Reuse key:** candidate × alternative × experimental context × evidence version

**Semantic work remaining:** Maps a documented intervention/readout to a plausible interpretation without trying to perform causal inference.

**Bypass Jev/Laya:** Use code for measured covariate associations; bypass if a curated intervention–readout relation already supplies the answer.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Direct/associative → flag for sensitivity analysis; against → preserve candidate with caveat; conflicting/U → deeper review.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** Evidence that an exposure can generate a signal does not show that it caused this cohort's signal.

**Proposed validation:** Expert relation labels plus sensitivity analyses; include confounded and experimentally manipulated examples.

### BM05 synthetic example

Input material: Patient metadata, adjusted/unadjusted analyses, cell-composition estimates and source excerpts about the named alternative.

SYNTHETIC: Candidate A rises after diagnosis. All severe cases received treatment T before sampling. E1 reports that T increases endogenous A in a controlled cell experiment. No within-cohort untreated severe comparison is available.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM05-E1",
      "source_id": "workbook-synthetic-BM05",
      "source_location": "Context_design!G10",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Candidate A rises after diagnosis. All severe cases received treatment T before sampling. E1 reports that T increases endogenous A in a controlled cell experiment. No within-cohort untreated severe comparison is available."
    }
  ],
  "context_fields": {
    "candidate_signal": "A rises after diagnosis",
    "alternative_explanation": "treatment T before sampling",
    "adjusted_results": null,
    "experimental_excerpt": "T increases endogenous A in a controlled cell experiment.",
    "setting": "All severe cases received T before sampling; no untreated severe comparison."
  },
  "missing_fields": [
    "adjusted_results"
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

**Missing-information variant:** Remove all information establishing candidate_signal from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 10, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="bm06"></a>

## BM06

**Question:** Does the antibody-validation evidence support recognition of the endogenous target in the proposed assay format?

**Decision unit:** One antibody/antibody pair × assay format

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Endogenous target in matching format | Specificity controls support endogenous-target recognition in the intended assay format. |
| 2 | Recombinant target only | Validation tests recombinant target without endogenous-target evidence. |
| 3 | Different assay application only | Evidence concerns another assay application without matching-format support. |
| 4 | Conflicting specificity evidence | Specificity findings for comparable reagents and conditions disagree. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Antibody validation methods + target/format metadata

**Minimum context:** Clone/pair identity; intended assay format and molecular target; endogenous versus recombinant material; reported specificity controls.

| Required field | Proposed supplier |
|---|---|
| `antibody_ids` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_format` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_form` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `tested_material` | Provided study metadata or source record; researcher confirms identity and meaning |
| `specificity_controls` | Provided study metadata or source record; researcher confirms identity and meaning |
| `evidence_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Match catalogue/clone IDs; retrieve paired-antibody documentation and validation passages; check numerical assay specifications separately.

**Optional cached LLM extraction:** Once per document, extract tested material, assay application and controls, retaining negative results. Do not convert 'validated' into a blanket quality label.

**Reuse key:** clone/pair × lot when relevant × application × document version

**Semantic work remaining:** Interprets validation language in relation to the actual assay application rather than treating all antibody evidence as interchangeable.

**Bypass Jev/Laya:** If a curated application/material/control record exists, let code assign the evidence category.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** BM: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep biological complementarity, assay evidence and clinical applicability as separate tags; combine with statistical panel results in code.

**Future routing:** Matching → retain for experimental qualification; recombinant/different → validation gap; conflicting/U → assay review.

**Expert follow-up:** Read full assay/cohort evidence, evaluate competing panels and plan endogenous-protein validation with an assay expert.

**Interpretation limit:** This labels the scope of evidence, not antibody quality or suitability for clinical testing.

**Proposed validation:** Assay-expert labels; include WB/IHC-only, recombinant-only and endogenous sandwich-assay examples.

### BM06 synthetic example

Input material: Manufacturer documentation, independent validation studies and planned capture/detection design.

SYNTHETIC: Proposed use is a sandwich ELISA for native protein A. E1 shows one antibody detecting a band in denatured lysate; E2 tests recombinant A in buffer. No matched capture/detection pair is evaluated.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "BM06",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-bm06",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "BM06-E1",
      "source_id": "workbook-synthetic-BM06",
      "source_location": "Context_design!G11",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Proposed use is a sandwich ELISA for native protein A. E1 shows one antibody detecting a band in denatured lysate; E2 tests recombinant A in buffer. No matched capture/detection pair is evaluated."
    }
  ],
  "context_fields": {
    "antibody_ids": null,
    "assay_format": "sandwich ELISA",
    "target_form": "native protein A",
    "tested_material": [
      "denatured lysate",
      "recombinant A in buffer"
    ],
    "specificity_controls": null,
    "evidence_excerpt": "No matched capture/detection pair is evaluated."
  },
  "missing_fields": [
    "antibody_ids",
    "specificity_controls"
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

**Missing-information variant:** Remove all information establishing antibody_ids from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 11, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC10335836/)
