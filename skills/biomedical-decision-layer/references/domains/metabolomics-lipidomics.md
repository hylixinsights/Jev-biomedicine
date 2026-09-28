# Metabolomics & lipidomics

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[ML01](#ml01) | [ML02](#ml02) | [ML03](#ml03) | [ML04](#ml04) | [ML05](#ml05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="ml01"></a>

## ML01

**Question:** Does the reference experiment distinguish this candidate from the remaining isomers?

**Decision unit:** One candidate among analytically plausible isomers

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Candidate-specific evidence | Matched analytical evidence distinguishes the candidate from the remaining isomers. |
| 2 | Chemical-class evidence only | Evidence identifies a chemical class without distinguishing its members. |
| 3 | Isomers remain unresolved | Comparable evidence leaves the remaining isomers indistinguishable. |
| 4 | Reference conditions are not comparable | Reference conditions prevent a valid comparison with the measured feature. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Specialist annotation results + reference-method text

**Minimum context:** Remaining candidate set; measured analytical evidence; reference standards/separation conditions and what the paper claims to distinguish.

| Required field | Proposed supplier |
|---|---|
| `feature_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `remaining_candidates` | Provided study metadata or source record; researcher confirms identity and meaning |
| `analytical_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `reference_conditions` | Provided study metadata or source record; researcher confirms identity and meaning |
| `standard_identity` | Provided study metadata or source record; researcher confirms identity and meaning |
| `method_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Perform feature detection, spectral scoring, formula/structure annotation and retention comparisons; retrieve unresolved reference-method statements.

**Optional cached LLM extraction:** Once per reference method, extract standards, separation and identification scope. Do not ask a text LLM to interpret raw peaks.

**Reuse key:** candidate set × method/library version × reference

**Semantic work remaining:** Determines the scope and comparability of textual identification evidence after numerical analysis.

**Bypass Jev/Laya:** If reference identity and analytical acceptance criteria are structured, evaluate them in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** ML: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Use contextual tags only within the analytically admissible candidate set; specialist analytical evidence sets the identity resolution.

**Future routing:** Specific → analytical-expert review; class/unresolved → retain ambiguous annotation; noncomparable/U → do not transfer identity.

**Expert follow-up:** An analytical chemist reviews raw spectra, standards and method comparability; run targeted validation where needed.

**Interpretation limit:** Biological plausibility must not override unresolved analytical identification.

**Proposed validation:** Analytical-chemist labels; test with known isomer mixtures and method-transfer cases.

### ML01 synthetic example

Input material: MS/MS and retention data, specialist-tool candidate table, reference-library metadata and methods.

SYNTHETIC: Candidates M1 and M2 have indistinguishable spectra in the pipeline. E1 used a different chromatographic method and reports only a shared chemical class; no matched standard resolves the pair.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "ML01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ml01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "ML01-E1",
      "source_id": "workbook-synthetic-ML01",
      "source_location": "Context_design!G36",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Candidates M1 and M2 have indistinguishable spectra in the pipeline. E1 used a different chromatographic method and reports only a shared chemical class; no matched standard resolves the pair."
    }
  ],
  "context_fields": {
    "feature_id": null,
    "remaining_candidates": [
      "M1",
      "M2"
    ],
    "analytical_results": "indistinguishable spectra",
    "reference_conditions": "different chromatographic method",
    "standard_identity": null,
    "method_excerpt": "Only shared chemical class reported; no matched standard resolves M1/M2."
  },
  "missing_fields": [
    "feature_id",
    "standard_identity"
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

**Missing-information variant:** Remove all information establishing feature_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 36, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://v6.docs.sirius-ms.io/)

<a id="ml02"></a>

## ML02

**Question:** What exposure evidence in the supplied history could account for this candidate compound?

**Decision unit:** One candidate compound × exposure passage

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Direct compound exposure documented | History and supplied ingredient records document exposure to the candidate itself. |
| 2 | Relevant precursor exposure documented | Exposure concerns a precursor with a documented relation to the candidate. |
| 3 | Relevant exposure explicitly denied | The relevant compound or precursor exposure is explicitly denied. |
| 4 | Exposure not documented | The reviewed history has no relevant exposure entry; this is not a denial. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** De-identified diet/medication/exposure text + ingredient/source records

**Minimum context:** Candidate identity; documented exposure passage; source/ingredient or precursor relation supplied explicitly; time context.

| Required field | Proposed supplier |
|---|---|
| `candidate_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `exposure_passage` | Source retrieval; optional cached factual extraction; human source check |
| `time_window` | Existing statistical or specialist output; record tool, version, units and QC |
| `ingredient_record` | Provided study metadata or source record; researcher confirms identity and meaning |
| `documented_precursor_relation` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Normalize known compound/product names and dates; match exact ingredients; retrieve only unresolved exposure descriptions.

**Optional cached LLM extraction:** Optionally curate product/ingredient descriptions once per source. No per-participant LLM summary is needed for short passages.

**Reuse key:** product/ingredient relation reusable; participant passage assembled locally

**Semantic work remaining:** Maps colloquial exposure descriptions to supplied chemical-source records when exact name matching fails.

**Bypass Jev/Laya:** Exact ingredient matches and time-window checks belong in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** ML: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Use contextual tags only within the analytically admissible candidate set; specialist analytical evidence sets the identity resolution.

**Future routing:** Direct/precursor → source-plausibility tag; denied/not documented → preserve origin uncertainty; U → targeted metadata review.

**Expert follow-up:** An analytical chemist reviews raw spectra, standards and method comparability; run targeted validation where needed.

**Interpretation limit:** Exposure evidence neither confirms chemical identity nor excludes endogenous production or unreported exposure.

**Proposed validation:** Expert exposure-relation labels; include missing histories and misleading brand names.

### ML02 synthetic example

Input material: Exposure questionnaires, medication records, food/product composition records and candidate annotations.

SYNTHETIC: M1 remains an analytical candidate. The participant reports a supplement by a local product name; its supplied ingredient record contains M1. The exposure timing is compatible with sample collection.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "ML02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ml02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "ML02-E1",
      "source_id": "workbook-synthetic-ML02",
      "source_location": "Context_design!G37",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: M1 remains an analytical candidate. The participant reports a supplement by a local product name; its supplied ingredient record contains M1. The exposure timing is compatible with sample collection."
    }
  ],
  "context_fields": {
    "candidate_id": "M1",
    "exposure_passage": "Participant reports a supplement by local product name.",
    "time_window": "Compatible with sample collection; exact interval not supplied.",
    "ingredient_record": "Supplied ingredient record contains M1.",
    "documented_precursor_relation": null
  },
  "missing_fields": [
    "documented_precursor_relation"
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

**Missing-information variant:** Remove all information establishing candidate_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 37, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://v6.docs.sirius-ms.io/)

<a id="ml03"></a>

## ML03

**Question:** Are the sample-handling conditions compatible with the documented transformation that could generate this feature?

**Decision unit:** One feature/candidate × documented handling transformation

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Conditions match the mechanism | Recorded handling meets the conditions for the specified chemical transformation. |
| 2 | Partial or uncertain condition match | Some necessary conditions match but others remain uncertain. |
| 3 | Conditions contradict the mechanism | Recorded conditions are incompatible with the specified transformation. |
| 4 | Competing transformations remain possible | Several transformations could explain the feature under the recorded conditions. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Collection/storage notes + documented chemical mechanism

**Minimum context:** Candidate/precursor identities; actual handling; experimentally documented transformation conditions; pipeline QC results.

| Required field | Proposed supplier |
|---|---|
| `candidate` | Provided study metadata or source record; researcher confirms identity and meaning |
| `precursor` | Provided study metadata or source record; researcher confirms identity and meaning |
| `handling_note` | Provided study metadata or source record; researcher confirms identity and meaning |
| `recorded_conditions` | Provided study metadata or source record; researcher confirms identity and meaning |
| `mechanism_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `control_results` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Calculate storage durations and technical associations; retrieve the documented transformation and original handling text.

**Optional cached LLM extraction:** Extract reagents, temperature, time and measured conversion from the source once; do not propose new chemistry.

**Reuse key:** chemical pair × handling mechanism × method/source version

**Semantic work remaining:** Matches partially structured laboratory descriptions to an existing transformation mechanism.

**Bypass Jev/Laya:** Use chemical condition rules when all relevant values and acceptance ranges are explicit.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** ML: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Use contextual tags only within the analytically admissible candidate set; specialist analytical evidence sets the identity resolution.

**Future routing:** Match/partial → handling-control review; contradiction → not supported by this mechanism; competing/U → keep alternatives.

**Expert follow-up:** An analytical chemist reviews raw spectra, standards and method comparability; run targeted validation where needed.

**Interpretation limit:** A compatible mechanism is not evidence that the conversion occurred in this sample.

**Proposed validation:** Chemist labels plus controlled fresh/stored sample experiments.

### ML03 synthetic example

Input material: Collection logs, freeze–thaw/storage metadata, standards/QC and handling-experiment methods.

SYNTHETIC: E1 documents conversion of precursor P to M1 during acidic storage. Sample notes describe acidification before a long storage interval; the laboratory has no fresh-sample control.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "ML03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ml03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "ML03-E1",
      "source_id": "workbook-synthetic-ML03",
      "source_location": "Context_design!G38",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: E1 documents conversion of precursor P to M1 during acidic storage. Sample notes describe acidification before a long storage interval; the laboratory has no fresh-sample control."
    }
  ],
  "context_fields": {
    "candidate": "M1",
    "precursor": "P",
    "handling_note": "acidification before long storage",
    "recorded_conditions": "acidified storage; duration not quantified",
    "mechanism_excerpt": "E1 documents P-to-M1 conversion during acidic storage.",
    "control_results": {
      "measurement_status": "not_measured",
      "basis": "No fresh-sample control."
    }
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

**Missing-information variant:** Remove all information establishing candidate from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 38, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://v6.docs.sirius-ms.io/)

<a id="ml04"></a>

## ML04

**Question:** Does the experiment demonstrate conversion along the proposed route or only a change in metabolite abundance?

**Decision unit:** One metabolic-route claim × supporting experiment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Tracer or direct conversion evidence | Tracer or conversion measurements directly test the proposed route. |
| 2 | Enzyme activity outside the target system | Enzyme activity supports conversion outside the target biological system. |
| 3 | Concentration association only | Evidence only relates concentrations without a conversion experiment. |
| 4 | Mixed evidence types | The supplied record contains multiple listed evidence types. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Metabolite results + tracer/enzymatic experimental descriptions

**Minimum context:** Proposed substrate–product route; labeling or activity design; compartment/system; quantitative results already analyzed.

| Required field | Proposed supplier |
|---|---|
| `proposed_route` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `system` | Provided study metadata or source record; researcher confirms identity and meaning |
| `tracer_design` | Provided study metadata or source record; researcher confirms identity and meaning |
| `computed_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `readout_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Fit isotopologue/flux models where appropriate and calculate abundance effects; retrieve the experiment's actual design.

**Optional cached LLM extraction:** Extract tracer/intervention and measured conversion/readout once per study; preserve compartment and labeling limitations.

**Reuse key:** route × experimental design × system × source version

**Semantic work remaining:** Classifies the inferential scope of the evidence rather than guessing metabolic flux from concentrations.

**Bypass Jev/Laya:** Use curated design/evidence labels if available; all flux estimation remains in specialist code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** ML: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Use contextual tags only within the analytically admissible candidate set; specialist analytical evidence sets the identity resolution.

**Future routing:** Conversion → retain route evidence; enzyme-other-system → transfer gap; concentration → association-only; mixed/U → review.

**Expert follow-up:** An analytical chemist reviews raw spectra, standards and method comparability; run targeted validation where needed.

**Interpretation limit:** Even labeling evidence requires specialist modeling; this question does not estimate flux.

**Proposed validation:** Experts label experiment scope; compare to independent tracer-based route validation.

### ML04 synthetic example

Input material: Metabolite tables, isotope/tracer-model outputs, enzymatic results and source methods.

SYNTHETIC: P decreases and M1 increases across patients. The cited study measures concentrations at one time point, without labeled substrate, an enzyme assay or a conversion experiment.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "ML04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ml04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "ML04-E1",
      "source_id": "workbook-synthetic-ML04",
      "source_location": "Context_design!G39",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: P decreases and M1 increases across patients. The cited study measures concentrations at one time point, without labeled substrate, an enzyme assay or a conversion experiment."
    }
  ],
  "context_fields": {
    "proposed_route": "P to M1",
    "system": "patient samples",
    "tracer_design": {
      "measurement_status": "not_measured"
    },
    "computed_results": "P decreases and M1 increases across patients",
    "readout_definition": "concentrations at one time point",
    "experiment_excerpt": "No labeled substrate, enzyme assay or conversion experiment."
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

**Missing-information variant:** Remove all information establishing proposed_route from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 39, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://v6.docs.sirius-ms.io/)

<a id="ml05"></a>

## ML05

**Question:** Does the described analytical evidence support the structural detail claimed for this lipid?

**Decision unit:** One lipid annotation claim × analytical method

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Claimed detail is explicitly resolved | The analytical method and observations resolve the claimed structural detail. |
| 2 | Only a coarser structure is resolved | Evidence supports only a broader structural description. |
| 3 | Method cannot support the claimed detail | Documented method limitations preclude the claimed detail. |
| 4 | Structural evidence conflicts | Relevant structural measurements disagree. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Lipid candidate/annotation level + analytical-method description

**Minimum context:** Exact structural claim; specialist assignment level; fragment/separation evidence; method's documented resolving scope.

| Required field | Proposed supplier |
|---|---|
| `lipid_claim` | Provided study metadata or source record; researcher confirms identity and meaning |
| `pipeline_annotation_level` | Provided study metadata or source record; researcher confirms identity and meaning |
| `resolving_evidence` | Provided study metadata or source record; researcher confirms identity and meaning |
| `standard` | Provided study metadata or source record; researcher confirms identity and meaning |
| `method_scope_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute and retain annotation level from validated lipid-identification rules; retrieve ambiguous claims from manuscripts or external datasets.

**Optional cached LLM extraction:** Extract what positional/chain detail the method actually distinguishes, once per protocol, without assigning structure from text alone.

**Reuse key:** structural claim level × analytical method × reference version

**Semantic work remaining:** Checks consistency between a narrative structural claim and the documented resolving power of the method.

**Bypass Jev/Laya:** Numerical structural-level assignment belongs to specialist tools; bypass when method capability is curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** ML: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Use contextual tags only within the analytically admissible candidate set; specialist analytical evidence sets the identity resolution.

**Future routing:** Resolved → retain claimed scope for expert checking; coarser/unsupported → downgrade annotation detail; conflict/U → review.

**Expert follow-up:** An analytical chemist reviews raw spectra, standards and method comparability; run targeted validation where needed.

**Interpretation limit:** The decision model must not invent acyl positions, stereochemistry or double-bond locations.

**Proposed validation:** Lipidomics experts label supported detail; include over-specific external annotations.

### ML05 synthetic example

Input material: Lipid feature table, specialist identification outputs, standards and methods describing chain/position resolution.

SYNTHETIC: A report names both acyl-chain positions. Its method section reports only a summed-composition match and states that positional isomers were not separated.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "ML05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ml05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "ML05-E1",
      "source_id": "workbook-synthetic-ML05",
      "source_location": "Context_design!G40",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A report names both acyl-chain positions. Its method section reports only a summed-composition match and states that positional isomers were not separated."
    }
  ],
  "context_fields": {
    "lipid_claim": "both acyl-chain positions",
    "pipeline_annotation_level": "summed composition",
    "resolving_evidence": "positional isomers not separated",
    "standard": null,
    "method_scope_excerpt": "Method reports only summed-composition match."
  },
  "missing_fields": [
    "standard"
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

**Missing-information variant:** Remove all information establishing lipid_claim from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 40, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://v6.docs.sirius-ms.io/)
