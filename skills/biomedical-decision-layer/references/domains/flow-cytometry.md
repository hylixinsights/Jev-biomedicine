# Flow cytometry

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[FC01](#fc01) | [FC02](#fc02) | [FC03](#fc03) | [FC04](#fc04)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="fc01"></a>

## FC01

**Question:** Does this gate's included-cell profile match the intended biological population?

**Decision unit:** One technically valid gate × intended population

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Matches intended population | The technically valid gate includes the intended biological population. |
| 2 | Captures only a relevant subset | The gate captures a relevant subset but omits other intended cells. |
| 3 | Includes a different population | The gate captures a different population. |
| 4 | Mixed target and off-target profile | The gate includes both target and off-target profiles. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Gate summaries + marker co-expression + population definition

**Minimum context:** Expert-defined target; technically valid gate ID/parent; included-cell marker patterns, uncertainty and experimental condition.

| Required field | Proposed supplier |
|---|---|
| `gate_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `parent_population` | Provided study metadata or source record; researcher confirms identity and meaning |
| `intended_population` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `included_profile` | Existing statistical or specialist output; record tool, version, units and QC |
| `controls_passed` | Provided study metadata or source record; researcher confirms identity and meaning |
| `condition` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Perform preprocessing/QC and fit candidate gates with specialist tools; summarize included/excluded cells and marker patterns. Never ask the model for polygon coordinates.

**Optional cached LLM extraction:** Optionally extract population definitions once from validated panel references; no LLM needed per event or gate.

**Reuse key:** population definition × panel/protocol version; profile per candidate gate

**Semantic work remaining:** Compares biological inclusion criteria with a candidate profile instead of calculating gate geometry or simply picking a template name.

**Bypass Jev/Laya:** If the target has a validated Boolean marker definition for this condition, evaluate it directly in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** FC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Evaluate each technically valid gate independently; code compares profiles and coverage. A cytometrist approves gate changes.

**Future routing:** Match → candidate for approved workflow; subset → label restricted coverage; different/mixed/U → adjust/review. Code compares all candidate-gate outputs.

**Expert follow-up:** Inspect FCS distributions, controls and excluded cells; a cytometrist chooses/approves the final gate.

**Interpretation limit:** Only technically valid gates enter this question; no autonomous final diagnostic gating.

**Proposed validation:** Cytometrist labels on gates, with event-level inclusion/exclusion audits; split by donor/instrument.

### FC01 synthetic example

Input material: FCS files, compensation/unmixing, control measurements, automated gating templates and population-reference descriptions.

SYNTHETIC: Target is all lineage-L cells, including activated cells. Gate G contains cells with constitutive L markers but restricts to a resting-state marker, excluding an activated L subset.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "FC01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-fc01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "FC01-E1",
      "source_id": "workbook-synthetic-FC01",
      "source_location": "Context_design!G50",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Target is all lineage-L cells, including activated cells. Gate G contains cells with constitutive L markers but restricts to a resting-state marker, excluding an activated L subset."
    }
  ],
  "context_fields": {
    "gate_id": "G",
    "parent_population": null,
    "intended_population": "all lineage-L cells, including activated cells",
    "included_profile": "constitutive L markers plus restriction to resting-state marker",
    "controls_passed": null,
    "condition": "activated L subset excluded"
  },
  "missing_fields": [
    "parent_population",
    "controls_passed"
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

**Missing-information variant:** Remove all information establishing gate_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 50, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/vignettes/openCyto/inst/doc/openCytoVignette.html)

<a id="fc02"></a>

## FC02

**Question:** Could the excluded-cell profile still belong to the target population under this experimental condition?

**Decision unit:** One candidate gate × excluded-cell profile

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Target-compatible excluded cells | Excluded cells retain features compatible with the target under the specified condition. |
| 2 | Exclusion supported by lineage evidence | Lineage evidence supports excluding these cells. |
| 3 | Mixed excluded populations | Excluded cells include more than one population with different target compatibility. |
| 4 | Condition-specific phenotype unresolved | Condition-dependent phenotype changes leave compatibility unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Excluded-cell summaries + condition-specific phenotype evidence

**Minimum context:** Excluded cluster's positive/negative markers; target definition; treatment/stimulation; supplied evidence on phenotype changes.

| Required field | Proposed supplier |
|---|---|
| `gate_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `excluded_profile` | Existing statistical or specialist output; record tool, version, units and QC |
| `target_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `condition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `marker_change_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `QC` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute excluded-cell co-expression and QC; apply known marker rules; retrieve evidence for the condition-sensitive marker.

**Optional cached LLM extraction:** Extract documented marker changes under the condition once per study; do not decide gate placement.

**Reuse key:** target × perturbation × marker evidence; excluded profile per gate

**Semantic work remaining:** Tests whether exclusion reflects a state-dependent phenotype rather than a different lineage.

**Bypass Jev/Laya:** Validated condition-specific marker rules and all event-count calculations stay in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** FC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Evaluate each technically valid gate independently; code compares profiles and coverage. A cytometrist approves gate changes.

**Future routing:** Compatible/mixed → cytometrist review of false exclusions; supported → retain exclusion; unresolved/U → no automatic discard.

**Expert follow-up:** Inspect FCS distributions, controls and excluded cells; a cytometrist chooses/approves the final gate.

**Interpretation limit:** Marker downregulation alone does not prove membership; additional markers and technical controls are required.

**Proposed validation:** Expert review of excluded events; quantify target-cell recall and rare-population loss.

### FC02 synthetic example

Input material: FCS data, gate membership, controls, reference panel definitions and condition-specific studies.

SYNTHETIC: Excluded cells retain the target's lineage markers but lose marker M after stimulation. E1 documents M downregulation in stimulated target cells; technical controls pass.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "FC02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-fc02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "FC02-E1",
      "source_id": "workbook-synthetic-FC02",
      "source_location": "Context_design!G51",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Excluded cells retain the target's lineage markers but lose marker M after stimulation. E1 documents M downregulation in stimulated target cells; technical controls pass."
    }
  ],
  "context_fields": {
    "gate_id": null,
    "excluded_profile": "target-lineage markers retained; M lost after stimulation",
    "target_definition": null,
    "condition": "stimulation",
    "marker_change_excerpt": "E1 documents M downregulation in stimulated target cells.",
    "QC": "technical controls pass"
  },
  "missing_fields": [
    "gate_id",
    "target_definition"
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

**Missing-information variant:** Remove all information establishing gate_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 51, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/vignettes/openCyto/inst/doc/openCytoVignette.html)

<a id="fc03"></a>

## FC03

**Question:** In this panel and condition, does the marker split distinguish lineage or a condition-dependent state?

**Decision unit:** One marker used to split a parent population

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Primarily lineage | The marker separates lineage identities in this parent population and condition. |
| 2 | Primarily condition-dependent state | The marker predominantly distinguishes a condition-dependent state. |
| 3 | Both lineage and state | Both lineage and state influence the marker split. |
| 4 | Role depends on unresolved context | The marker role cannot be resolved for the specified context. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Panel metadata + parent-population profile + marker evidence

**Minimum context:** Parent population; marker/clone; condition; evidence describing what marker positivity means within this parent.

| Required field | Proposed supplier |
|---|---|
| `parent_population` | Provided study metadata or source record; researcher confirms identity and meaning |
| `marker` | Existing statistical or specialist output; record tool, version, units and QC |
| `clone` | Provided study metadata or source record; researcher confirms identity and meaning |
| `condition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `expression_summary` | Provided study metadata or source record; researcher confirms identity and meaning |
| `marker_role_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute expression patterns and reference mappings; retrieve condition-specific marker descriptions.

**Optional cached LLM extraction:** Extract marker role and the population in which it was tested once per source, not a general role stripped of context.

**Reuse key:** marker × parent population × condition × evidence version

**Semantic work remaining:** Distinguishes biological identity from a state-dependent split when the same marker serves different purposes.

**Bypass Jev/Laya:** Use curated panel-specific marker-role mappings directly.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** FC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Evaluate each technically valid gate independently; code compares profiles and coverage. A cytometrist approves gate changes.

**Future routing:** Lineage/state/both → annotate gate meaning; unresolved/U → panel-expert review.

**Expert follow-up:** Inspect FCS distributions, controls and excluded cells; a cytometrist chooses/approves the final gate.

**Interpretation limit:** This does not determine the fluorescence threshold, compensation or optimal staining panel.

**Proposed validation:** Cytometrist labels with marker/parent/condition held out where feasible.

### FC03 synthetic example

Input material: Panel annotation, FCS-derived profiles and reference-method/marker-function passages.

SYNTHETIC: In the selected parent lineage, marker M appears after stimulation in multiple subtypes. The panel proposal treats M positivity as a new lineage.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "FC03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-fc03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "FC03-E1",
      "source_id": "workbook-synthetic-FC03",
      "source_location": "Context_design!G52",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: In the selected parent lineage, marker M appears after stimulation in multiple subtypes. The panel proposal treats M positivity as a new lineage."
    }
  ],
  "context_fields": {
    "parent_population": "selected parent lineage",
    "marker": "M",
    "clone": null,
    "condition": "stimulation",
    "expression_summary": "M appears in multiple subtypes after stimulation",
    "marker_role_excerpt": "Panel proposal treats M positivity as a new lineage."
  },
  "missing_fields": [
    "clone"
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

**Missing-information variant:** Remove all information establishing parent_population from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 52, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/vignettes/openCyto/inst/doc/openCytoVignette.html)

<a id="fc04"></a>

## FC04

**Question:** Does the supplied evidence support a reagent-related explanation for apparent marker loss?

**Decision unit:** One marker-loss exclusion × reagent/perturbation evidence

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Direct reagent-interference evidence | A matched reagent experiment directly supports interference. |
| 2 | Plausible but untested interference | A reagent mechanism is plausible but lacks a matching test. |
| 3 | Evidence against interference | A relevant control experiment argues against interference. |
| 4 | Conflicting reagent evidence | Comparable reagent experiments give incompatible evidence. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Clone/epitope metadata + treatment/digestion notes + validation text

**Minimum context:** Detection clone; relevant treatment or digestion; observed loss; technical controls; documented masking/cleavage/interference mechanism.

| Required field | Proposed supplier |
|---|---|
| `detection_clone` | Provided study metadata or source record; researcher confirms identity and meaning |
| `epitope` | Provided study metadata or source record; researcher confirms identity and meaning |
| `protocol` | Provided study metadata or source record; researcher confirms identity and meaning |
| `treatment` | Provided study metadata or source record; researcher confirms identity and meaning |
| `observed_loss` | Provided study metadata or source record; researcher confirms identity and meaning |
| `interference_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `controls` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Check clone IDs, compensation/unmixing and staining QC; retrieve evidence relating the treatment/protocol to detection of the same epitope.

**Optional cached LLM extraction:** Extract interference mechanism and tested clone/material once per source; do not infer an unknown epitope interaction.

**Reuse key:** clone × treatment/protocol × validation-source version

**Semantic work remaining:** Relates assay-specific evidence to apparent phenotype loss when a marker threshold alone would mislead.

**Bypass Jev/Laya:** Exact, curated interference rules should bypass the decision model; all fluorescence QC remains upstream.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** FC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Evaluate each technically valid gate independently; code compares profiles and coverage. A cytometrist approves gate changes.

**Future routing:** Direct/plausible/conflicting → review exclusion and alternate clone; against → no support for this mechanism; U → obtain validation.

**Expert follow-up:** Inspect FCS distributions, controls and excluded cells; a cytometrist chooses/approves the final gate.

**Interpretation limit:** An interference hypothesis does not justify automatic relabeling or changing a clinical gate.

**Proposed validation:** Expert assay-interference labels plus alternate-clone/control experiments.

### FC04 synthetic example

Input material: FCS/control output, reagent identifiers, sample protocols and relevant validation experiments.

SYNTHETIC: Gate G excludes cells after signal M disappears. E1 shows that treatment T blocks binding of the same detection clone without reducing total target protein. T was present during sampling.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "FC04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-fc04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "FC04-E1",
      "source_id": "workbook-synthetic-FC04",
      "source_location": "Context_design!G53",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Gate G excludes cells after signal M disappears. E1 shows that treatment T blocks binding of the same detection clone without reducing total target protein. T was present during sampling."
    }
  ],
  "context_fields": {
    "detection_clone": "same clone as E1; name not supplied",
    "epitope": null,
    "protocol": "T present during sampling",
    "treatment": "T",
    "observed_loss": "M signal disappears",
    "interference_excerpt": "T blocks binding of the same detection clone without reducing total target protein.",
    "controls": null
  },
  "missing_fields": [
    "epitope",
    "controls"
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

**Missing-information variant:** Remove all information establishing detection_clone from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 53, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/vignettes/openCyto/inst/doc/openCytoVignette.html)
- [Background source 2](https://pmc.ncbi.nlm.nih.gov/articles/PMC10335836/)
