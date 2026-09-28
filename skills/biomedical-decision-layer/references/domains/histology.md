# Histology

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[HI01](#hi01) | [HI02](#hi02) | [HI03](#hi03) | [HI04](#hi04)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="hi01"></a>

## HI01

**Question:** Does the region description contain the tissue compartment required to test the stated hypothesis?

**Decision unit:** One candidate region × required tissue compartment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Required compartment represented | The described region contains the compartment required by the hypothesis. |
| 2 | Adjacent compartment only | Only an adjacent compartment is represented. |
| 3 | Different compartment | The region belongs to a different compartment. |
| 4 | Mixed required and other compartments | The region mixes the required compartment with others. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Region-level descriptors + compartment labels + research definition

**Minimum context:** Hypothesis; exact compartment definition; region morphology/label uncertainty; precomputed cell and tissue fractions.

| Required field | Proposed supplier |
|---|---|
| `region_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `hypothesis` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `required_compartment` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `morphology_descriptor` | Provided study metadata or source record; researcher confirms identity and meaning |
| `compartment_fractions` | Existing statistical or specialist output; record tool, version, units and QC |
| `uncertainty` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Detect regions, segment and measure tissue/cell fractions; remove technical failures; provide labeled descriptors, not pixels or raw embeddings.

**Optional cached LLM extraction:** A validated vision model may describe morphology when no suitable labels exist. Preserve image coordinates and verify descriptor quality separately.

**Reuse key:** slide/region × segmentation/descriptor version × hypothesis definition

**Semantic work remaining:** Matches a biological compartment definition to descriptive region language when exact ontology labels are unavailable.

**Bypass Jev/Laya:** If validated compartment masks and numerical coverage criteria answer the question, use code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** HI: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine tissue adequacy, artifact and feature-coverage tags within a prespecified sampling plan; update selected sets between rounds.

**Future routing:** Required/mixed → retain with compartment annotation; adjacent/different → different stratum; U → pathologist review.

**Expert follow-up:** A pathologist inspects the region image and sampling purpose; revise ROI or assay plan as needed.

**Interpretation limit:** The model consumes descriptors; it does not independently verify the slide or diagnose disease.

**Proposed validation:** Pathologist labels grounded in the actual region image; split by patient/slide.

### HI01 synthetic example

Input material: Whole-slide image, segmentation/compartment outputs, pathologist annotations and study sampling plan.

SYNTHETIC: The question concerns epithelial injury. Region R has stromal inflammation near an epithelial boundary, but the region mask excludes epithelial tissue.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "HI01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-hi01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "HI01-E1",
      "source_id": "workbook-synthetic-HI01",
      "source_location": "Context_design!G54",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The question concerns epithelial injury. Region R has stromal inflammation near an epithelial boundary, but the region mask excludes epithelial tissue."
    }
  ],
  "context_fields": {
    "region_id": "R",
    "hypothesis": "epithelial injury",
    "required_compartment": "epithelium",
    "morphology_descriptor": "stromal inflammation near epithelial boundary; mask excludes epithelium",
    "compartment_fractions": null,
    "uncertainty": null
  },
  "missing_fields": [
    "compartment_fractions",
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

**Missing-information variant:** Remove all information establishing region_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 54, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://qupath.readthedocs.io/en/stable/docs/intro/about.html)

<a id="hi02"></a>

## HI02

**Question:** Does the region description support a biological pattern, a processing artifact, or both?

**Decision unit:** One described anomaly × artifact evidence

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Biological pattern supported | Validated descriptors and context support a biological pattern. |
| 2 | Processing artifact supported | Descriptors and processing evidence support an artifact. |
| 3 | Both remain plausible | Both biological and processing explanations remain compatible. |
| 4 | Description is internally inconsistent | The supplied descriptors contradict each other. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Image-analysis descriptors + processing/QC notes + artifact definitions

**Minimum context:** Anomaly description; slide preparation; validated artifact descriptors; control/adjacent-region context.

| Required field | Proposed supplier |
|---|---|
| `region_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `anomaly_descriptor` | Provided study metadata or source record; researcher confirms identity and meaning |
| `processing_notes` | Provided study metadata or source record; researcher confirms identity and meaning |
| `artifact_criteria` | Provided study metadata or source record; researcher confirms identity and meaning |
| `QC` | Existing statistical or specialist output; record tool, version, units and QC |
| `adjacent_context` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Apply validated numerical image-QC rules first; retrieve residual ambiguous descriptors with region coordinates.

**Optional cached LLM extraction:** Optional vision model describes the observable morphology, not the final artifact category; no requirement to caption every tile.

**Reuse key:** artifact-definition library reusable; region descriptors per slide

**Semantic work remaining:** Interprets residual morphology/processing descriptions that did not meet a reliable automatic QC rule.

**Bypass Jev/Laya:** Known focus, fold or stain thresholds and validated image classifiers should answer directly.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** HI: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine tissue adequacy, artifact and feature-coverage tags within a prespecified sampling plan; update selected sets between rounds.

**Future routing:** Artifact → technical-review/reacquisition queue; biology → biological-review queue; both/inconsistent/U → pathologist sees the image.

**Expert follow-up:** A pathologist inspects the region image and sampling purpose; revise ROI or assay plan as needed.

**Interpretation limit:** Do not discard a rare lesion automatically because its descriptor resembles an artifact.

**Proposed validation:** Pathologist image labels; audit all rare-pattern losses and upstream descriptor errors.

### HI02 synthetic example

Input material: Slide imagery, focus/fold/stain QC, pathologist/vision descriptions and preparation logs.

SYNTHETIC: A dense dark band aligns with a recorded tissue fold and contains duplicated contours. No corresponding lesion is reported in the adjacent unfolded region.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "HI02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-hi02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "HI02-E1",
      "source_id": "workbook-synthetic-HI02",
      "source_location": "Context_design!G55",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A dense dark band aligns with a recorded tissue fold and contains duplicated contours. No corresponding lesion is reported in the adjacent unfolded region."
    }
  ],
  "context_fields": {
    "region_id": null,
    "anomaly_descriptor": "dense dark band with duplicated contours",
    "processing_notes": "recorded tissue fold aligned with the band",
    "artifact_criteria": null,
    "QC": null,
    "adjacent_context": "No corresponding lesion reported in adjacent unfolded region."
  },
  "missing_fields": [
    "region_id",
    "artifact_criteria",
    "QC"
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

**Missing-information variant:** Remove all information establishing region_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 55, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://qupath.readthedocs.io/en/stable/docs/intro/about.html)

<a id="hi03"></a>

## HI03

**Question:** Does this region add a histological feature not represented in the selected regions?

**Decision unit:** One candidate region × fixed selected-region set

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Distinct lesion pattern | The candidate adds a lesion pattern absent from the fixed selected set. |
| 2 | Distinct tissue interface only | It adds an unrepresented tissue interface without a distinct lesion pattern. |
| 3 | Same represented features | All relevant features are already represented in the fixed set. |
| 4 | Mixed distinct and repeated features | It adds some distinct features and repeats others. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Region feature descriptions + selected-set inventory

**Minimum context:** Sampling objective; candidate morphology; concise feature inventory for already selected regions, fixed before parallel evaluation.

| Required field | Proposed supplier |
|---|---|
| `candidate_region` | Provided study metadata or source record; researcher confirms identity and meaning |
| `morphology_features` | Provided study metadata or source record; researcher confirms identity and meaning |
| `selected_set_snapshot` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `represented_features` | Provided study metadata or source record; researcher confirms identity and meaning |
| `sampling_objective` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute embedding/feature redundancy and coverage first; retrieve only unresolved semantic feature descriptions. Keep the selected-set snapshot fixed within a batch.

**Optional cached LLM extraction:** Optionally extract or normalize feature descriptions once per distinct region; do not compare every pixel pair with an LLM.

**Reuse key:** region descriptor × selected-set version; re-evaluate only when the set changes

**Semantic work remaining:** Compares descriptive lesion/interface coverage when numerical similarity cannot establish biological redundancy.

**Bypass Jev/Laya:** Validated feature labels, set overlap and distance-based sampling belong in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** HI: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine tissue adequacy, artifact and feature-coverage tags within a prespecified sampling plan; update selected sets between rounds.

**Future routing:** Distinct/interface/mixed → candidate for coverage-aware sampling; same → redundancy tag; U → review. Code updates the selected set between rounds.

**Expert follow-up:** A pathologist inspects the region image and sampling purpose; revise ROI or assay plan as needed.

**Interpretation limit:** Pure novelty-based selection biases prevalence estimates; retain prespecified/random sampling where needed.

**Proposed validation:** Pathologist feature-coverage labels; test representativeness and lesion recall, not just diversity.

### HI03 synthetic example

Input material: Region descriptors, feature/compartment labels, spatial coordinates and sampling design.

SYNTHETIC: Selected regions cover the tumor core. Candidate R has the same tumor morphology but includes the tumor–stroma interface, which is absent from the selected set.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "HI03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-hi03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "HI03-E1",
      "source_id": "workbook-synthetic-HI03",
      "source_location": "Context_design!G56",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Selected regions cover the tumor core. Candidate R has the same tumor morphology but includes the tumor–stroma interface, which is absent from the selected set."
    }
  ],
  "context_fields": {
    "candidate_region": "R",
    "morphology_features": [
      "same tumor morphology",
      "tumor–stroma interface"
    ],
    "selected_set_snapshot": "selected regions cover tumor core",
    "represented_features": [
      "tumor core"
    ],
    "sampling_objective": null
  },
  "missing_fields": [
    "sampling_objective"
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

**Missing-information variant:** Remove all information establishing candidate_region from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 56, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://qupath.readthedocs.io/en/stable/docs/intro/about.html)

<a id="hi04"></a>

## HI04

**Question:** Does the region annotation support assigning the planned measurement to the biological compartment named in the hypothesis?

**Decision unit:** One region annotation × downstream quantification claim

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Compartment attribution supported | Annotation and measurement resolution support attribution to the intended compartment. |
| 2 | Mixed compartments prevent attribution | Mixed compartments cannot be separated at the measurement resolution. |
| 3 | Annotation targets another compartment | The annotation identifies a compartment different from the intended one. |
| 4 | Attribution remains unresolved | Available annotation and method information leave attribution unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** ROI descriptors + assay localization + measurement plan

**Minimum context:** Claimed cell/compartment; actual region mask and cell content; spatial resolution of the proposed assay; attribution requirements.

| Required field | Proposed supplier |
|---|---|
| `region_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `intended_compartment` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `ROI_composition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_resolution` | Provided study metadata or source record; researcher confirms identity and meaning |
| `measurement_mask` | Existing statistical or specialist output; record tool, version, units and QC |
| `protocol_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute compartment composition and resolution/overlap; attach any validated cell-specific measurement masks.

**Optional cached LLM extraction:** Extract what the assay measures and its localization constraints once per protocol; a vision model is optional only for missing descriptors.

**Reuse key:** assay protocol × ROI/segmentation version × attribution definition

**Semantic work remaining:** Checks the meaning of an attribution claim against the described sampling/measurement unit.

**Bypass Jev/Laya:** If segmentation and assay resolution fully determine attribution, evaluate it in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** HI: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine tissue adequacy, artifact and feature-coverage tags within a prespecified sampling plan; update selected sets between rounds.

**Future routing:** Supported → retain measurement plan; mixed/other → revise ROI or interpretation; unresolved/U → pathology/assay review.

**Expert follow-up:** A pathologist inspects the region image and sampling purpose; revise ROI or assay plan as needed.

**Interpretation limit:** Region-level intensity cannot be assigned to a cell type solely because that type is present.

**Proposed validation:** Pathologist/assay-expert labels; compare region-level and cell-resolved measurements.

### HI04 synthetic example

Input material: Image masks, segmentation, IHC/ISH or spatial-assay design, ROI labels and sampling protocol.

SYNTHETIC: The study plans to attribute signal to tumor cells, but the selected bulk ROI contains tumor, immune cells and stroma; no cell-specific measurement mask or separation is available.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "HI04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-hi04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "HI04-E1",
      "source_id": "workbook-synthetic-HI04",
      "source_location": "Context_design!G57",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The study plans to attribute signal to tumor cells, but the selected bulk ROI contains tumor, immune cells and stroma; no cell-specific measurement mask or separation is available."
    }
  ],
  "context_fields": {
    "region_id": null,
    "intended_compartment": "tumor cells",
    "ROI_composition": [
      "tumor",
      "immune cells",
      "stroma"
    ],
    "assay_resolution": "bulk ROI",
    "measurement_mask": {
      "availability": "no cell-specific mask or separation"
    },
    "protocol_excerpt": "Signal is planned to be attributed to tumor cells despite mixed bulk ROI."
  },
  "missing_fields": [
    "region_id"
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

**Missing-information variant:** Remove all information establishing region_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 57, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://qupath.readthedocs.io/en/stable/docs/intro/about.html)
