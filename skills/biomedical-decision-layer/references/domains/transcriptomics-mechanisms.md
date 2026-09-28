# Transcriptomics — mechanisms

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[TH01](#th01) | [TH02](#th02) | [TH03](#th03) | [TH04](#th04) | [TH05](#th05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="th01"></a>

## TH01

**Question:** How does this perturbation experiment bear on the proposed gene–process relationship?

**Decision unit:** One gene–process hypothesis × perturbation experiment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Supports the relationship | The measured perturbation outcome agrees with the specified gene–process hypothesis. |
| 2 | Contradicts the relationship | The measured perturbation outcome opposes the specified hypothesis. |
| 3 | Mixed endpoint evidence | Relevant endpoints support different interpretations under the same hypothesis. |
| 4 | Does not test the relationship | The experiment addresses another relationship or endpoint. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Perturbation methods + computed effects + endpoint definitions

**Minimum context:** Explicit directional hypothesis; target manipulation; experimental system; biological meaning of each endpoint; numerical effects already computed.

| Required field | Proposed supplier |
|---|---|
| `hypothesis` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `intervention` | Provided study metadata or source record; researcher confirms identity and meaning |
| `system` | Provided study metadata or source record; researcher confirms identity and meaning |
| `endpoint_definitions` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `computed_effects` | Existing statistical or specialist output; record tool, version, units and QC |
| `source_spans` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute effect direction and uncertainty; associate each endpoint with its source passage and intervention arm.

**Optional cached LLM extraction:** Extract what was manipulated and measured, including controls and rescue conditions. Preserve null results rather than only the abstract's main finding.

**Reuse key:** experiment × comparison × candidate × source version

**Semantic work remaining:** Relates heterogeneous endpoint meanings to a stated mechanism; it does not calculate significance or invent a mechanism.

**Bypass Jev/Laya:** Use code for sign comparisons if endpoint meaning and causal relation are already explicitly curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** TH: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Tag each experiment separately. Aggregate independent evidence groups without vote-counting duplicated studies; preserve contradictions.

**Future routing:** Support → mechanistic-review queue; contradiction/mixed → retain in contradiction queue; untested/U → identify missing experiment.

**Expert follow-up:** Review full experiments, reconcile competing mechanisms and design perturbation/rescue studies with a domain expert.

**Interpretation limit:** A supporting experiment is context-specific; it is not proof that inhibition treats the disease.

**Proposed validation:** Experts label each hypothesis–experiment pair; test on unseen studies and mechanisms.

### TH01 synthetic example

Input material: Differential-expression results, perturbation tables, controls and full methods/results passages.

SYNTHETIC: Hypothesis: reducing A protects the epithelial barrier. A knockdown lowers an inflammation reporter but barrier permeability increases. Both outcomes are measured under the same perturbation.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "TH01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-th01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "TH01-E1",
      "source_id": "workbook-synthetic-TH01",
      "source_location": "Context_design!G12",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Hypothesis: reducing A protects the epithelial barrier. A knockdown lowers an inflammation reporter but barrier permeability increases. Both outcomes are measured under the same perturbation."
    }
  ],
  "context_fields": {
    "hypothesis": "reducing A protects the epithelial barrier",
    "intervention": "A knockdown",
    "system": null,
    "endpoint_definitions": [
      "inflammation reporter",
      "barrier permeability"
    ],
    "computed_effects": {
      "inflammation_reporter_direction": "lower",
      "barrier_permeability_direction": "higher"
    },
    "source_spans": [
      "Both outcomes measured under the same perturbation."
    ]
  },
  "missing_fields": [
    "system"
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

**Missing-information variant:** Remove all information establishing hypothesis from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 12, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="th02"></a>

## TH02

**Question:** In what biological context was this specific relationship experimentally tested?

**Decision unit:** One proposed relationship × retrieved evidence item

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Target disease and cell state | The exact relationship is experimentally tested in the target disease and cell state. |
| 2 | Related context only | The exact relationship is tested only in a related biological context. |
| 3 | Mentioned without an experimental test | The relationship is discussed without a matching experiment. |
| 4 | No matching relationship in retrieved item | The retrieved item contains no matching relationship; this says nothing about the whole literature. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Experiment excerpts + disease/cell-state metadata

**Minimum context:** Precisely stated relationship; target context; methods/results for one retrieved item; retrieval coverage record.

| Required field | Proposed supplier |
|---|---|
| `relationship` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_context` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `observed_context` | Provided study metadata or source record; researcher confirms identity and meaning |
| `retrieval_coverage` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Retrieve candidate–process passages and experiment descriptions; log query, date, corpus and accessible full-text coverage.

**Optional cached LLM extraction:** Extract disease/model/state and what was experimentally tested, with provenance; do not declare novelty.

**Reuse key:** relationship × experiment × context × corpus snapshot

**Semantic work remaining:** Separates an actual test in the target context from a narrative mention or a related model.

**Bypass Jev/Laya:** If experiment and context tags are curated, use exact/ontology-aware matching in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** TH: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Tag each experiment separately. Aggregate independent evidence groups without vote-counting duplicated studies; preserve contradictions.

**Future routing:** Target → known-context evidence; related/mention/no match → potential evidence gap, not novelty; U → retrieve missing methods.

**Expert follow-up:** Review full experiments, reconcile competing mechanisms and design perturbation/rescue studies with a domain expert.

**Interpretation limit:** No matching retrieved evidence is not proof that a relationship is new.

**Proposed validation:** Experts annotate test versus mention and context transfer; evaluate retrieval recall separately.

### TH02 synthetic example

Input material: Search results, permitted full texts, supplementary experiments and structured context annotations.

SYNTHETIC: Target is A-mediated barrier injury in psoriatic skin. E1 perturbs A in intestinal epithelium. A psoriasis review cites E1 but reports no skin experiment.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "TH02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-th02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "TH02-E1",
      "source_id": "workbook-synthetic-TH02",
      "source_location": "Context_design!G13",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Target is A-mediated barrier injury in psoriatic skin. E1 perturbs A in intestinal epithelium. A psoriasis review cites E1 but reports no skin experiment."
    }
  ],
  "context_fields": {
    "relationship": "A-mediated barrier injury",
    "target_context": "psoriatic skin",
    "experiment_excerpt": "E1 perturbs A in intestinal epithelium.",
    "observed_context": "intestinal epithelium",
    "retrieval_coverage": "One experiment E1 and a psoriasis review citing E1; no skin experiment reported in the review."
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

**Missing-information variant:** Remove all information establishing relationship from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 13, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="th03"></a>

## TH03

**Question:** Does the reported manipulation change a disease-relevant phenotype or only a molecular surrogate?

**Decision unit:** One perturbation × disease-relevant phenotype

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Disease-relevant phenotype measured | The experiment measures the defined disease-relevant phenotype. |
| 2 | Molecular surrogate only | Only a molecular proxy is measured. |
| 3 | Both phenotype and surrogate measured | Both the defined phenotype and a molecular proxy are measured. |
| 4 | Different biological endpoint | The measured endpoint is biologically different from the target phenotype. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Experimental endpoint descriptions + prespecified phenotype definition

**Minimum context:** Research question's phenotype definition; actual assay readouts; intervention/control descriptions.

| Required field | Proposed supplier |
|---|---|
| `target_phenotype` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `endpoints` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `intervention` | Provided study metadata or source record; researcher confirms identity and meaning |
| `results_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Retrieve endpoints and compute effect sizes; attach existing endpoint ontology labels when available.

**Optional cached LLM extraction:** Extract measured endpoints and their assay definitions once per experiment; do not reinterpret marker changes as functional rescue.

**Reuse key:** endpoint/assay × experiment version

**Semantic work remaining:** Determines whether the described readout actually addresses the biological question when labels are vague.

**Bypass Jev/Laya:** If endpoints already map to a curated phenotype/surrogate ontology for this question, use code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** TH: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Tag each experiment separately. Aggregate independent evidence groups without vote-counting duplicated studies; preserve contradictions.

**Future routing:** Phenotype/both → retain for functional review; surrogate/different → note validation gap; U → retrieve methods.

**Expert follow-up:** Review full experiments, reconcile competing mechanisms and design perturbation/rescue studies with a domain expert.

**Interpretation limit:** A disease-related molecular marker is not automatically a functional or clinical endpoint.

**Proposed validation:** Blinded endpoint-scope labels by domain experts.

### TH03 synthetic example

Input material: Functional-assay results, figures/tables where accessible, methods and prespecified study objective.

SYNTHETIC: Intended phenotype is barrier leakage. E1 measures two inflammatory transcripts after A inhibition but does not measure permeability, tissue injury or barrier function.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "TH03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-th03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "TH03-E1",
      "source_id": "workbook-synthetic-TH03",
      "source_location": "Context_design!G14",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Intended phenotype is barrier leakage. E1 measures two inflammatory transcripts after A inhibition but does not measure permeability, tissue injury or barrier function."
    }
  ],
  "context_fields": {
    "target_phenotype": "barrier leakage",
    "endpoints": [
      "two inflammatory transcripts"
    ],
    "assay_definition": null,
    "intervention": "A inhibition",
    "results_excerpt": "Permeability, tissue injury and barrier function were not measured."
  },
  "missing_fields": [
    "assay_definition"
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

**Missing-information variant:** Remove all information establishing target_phenotype from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 14, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="th04"></a>

## TH04

**Question:** Does the experiment distinguish the proposed target's contribution from general cytotoxicity?

**Decision unit:** One intervention result × proposed target attribution

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Target-specific contribution supported | Controls, engagement and orthogonal or rescue evidence separate target contribution from general toxicity. |
| 2 | Cytotoxicity remains unresolved | Viability or specificity controls do not resolve a cytotoxic explanation. |
| 3 | Effect coincides with broad cell loss | The relevant effect occurs alongside broad cell loss. |
| 4 | No target-specificity test | The report contains no experiment designed to test target specificity. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Perturbation results + viability/selectivity/control descriptions

**Minimum context:** Target engagement, viability, dose conditions and orthogonal or rescue controls supplied together.

| Required field | Proposed supplier |
|---|---|
| `intervention` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_engagement` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `viability_by_dose` | Provided study metadata or source record; researcher confirms identity and meaning |
| `control_design` | Provided study metadata or source record; researcher confirms identity and meaning |
| `rescue_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Calculate viability/effect summaries by dose and list controls; retrieve the description linking each control to the result.

**Optional cached LLM extraction:** Extract control design and what rescue or orthogonal perturbation changed, without assigning causality.

**Reuse key:** intervention × dose condition × experiment

**Semantic work remaining:** Interprets whether the control design separates target biology from nonspecific damage.

**Bypass Jev/Laya:** Use code for viability thresholds; curated control sufficiency rules should bypass this question.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** TH: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Tag each experiment separately. Aggregate independent evidence groups without vote-counting duplicated studies; preserve contradictions.

**Future routing:** Specific → retain evidence; unresolved/cell loss/no test → require specificity review; U → obtain control data.

**Expert follow-up:** Review full experiments, reconcile competing mechanisms and design perturbation/rescue studies with a domain expert.

**Interpretation limit:** No short semantic answer establishes drug selectivity, safety or therapeutic benefit.

**Proposed validation:** Expert control-design labels; include concordant and discordant rescue experiments.

### TH04 synthetic example

Input material: Dose-response tables, viability assays, perturbation methods and rescue/orthogonal-intervention passages.

SYNTHETIC: Compound T reduces the inflammatory readout and total viable cells at the same dose. The report lacks an orthogonal genetic perturbation or rescue experiment.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "TH04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-th04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "TH04-E1",
      "source_id": "workbook-synthetic-TH04",
      "source_location": "Context_design!G15",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Compound T reduces the inflammatory readout and total viable cells at the same dose. The report lacks an orthogonal genetic perturbation or rescue experiment."
    }
  ],
  "context_fields": {
    "intervention": "compound T",
    "target_engagement": null,
    "viability_by_dose": "Readout and viable-cell count fall at the same reported dose; numerical dose not supplied.",
    "control_design": "No orthogonal genetic perturbation or rescue experiment reported.",
    "rescue_excerpt": "No rescue experiment reported."
  },
  "missing_fields": [
    "target_engagement"
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

**Missing-information variant:** Remove all information establishing intervention from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 15, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="th05"></a>

## TH05

**Question:** Does the supplied experiment place the candidate response before injury or only after injury is established?

**Decision unit:** One candidate expression change × source timing evidence

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Response precedes measured injury | The measured response occurs before the defined injury onset at adequate time resolution. |
| 2 | Response follows established injury | The response is observed only after injury is established. |
| 3 | Different timing across conditions | Relative timing differs across reported experimental conditions. |
| 4 | Timing is not experimentally resolved | The experiment cannot order response and injury despite reporting timing information. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Time-course summary + narrative event/endpoint definitions

**Minimum context:** Definition of injury onset; time points and computed trajectories; source wording on intervention and onset.

| Required field | Proposed supplier |
|---|---|
| `response_times` | Provided study metadata or source record; researcher confirms identity and meaning |
| `injury_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `injury_onset` | Provided study metadata or source record; researcher confirms identity and meaning |
| `temporal_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `time_resolution` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute onset intervals and time alignment when numeric times exist; retrieve ambiguous descriptions of injury establishment.

**Optional cached LLM extraction:** Extract temporal anchors from methods once per study; do not infer that temporal order implies causality.

**Reuse key:** study × endpoint × time-course definition

**Semantic work remaining:** Maps phrases such as 'after establishment' to the study's temporal design when timestamps alone are incomplete.

**Bypass Jev/Laya:** If both response and injury timing are explicit, compute the relation in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** TH: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Tag each experiment separately. Aggregate independent evidence groups without vote-counting duplicated studies; preserve contradictions.

**Future routing:** Precedes → temporal-support tag; follows → consequence-compatible tag; different/unresolved/U → retain temporal uncertainty.

**Expert follow-up:** Review full experiments, reconcile competing mechanisms and design perturbation/rescue studies with a domain expert.

**Interpretation limit:** Temporal precedence does not distinguish a causal driver from a correlated early response.

**Proposed validation:** Experts annotate temporal statements; no causal-driver label from this question.

### TH05 synthetic example

Input material: Time-course counts/measurements, onset metadata and study methods.

SYNTHETIC: A increases at the first sampled time point. E1 states that tissue injury was already present when sampling began; no pre-injury specimens were collected.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "TH05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-th05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "TH05-E1",
      "source_id": "workbook-synthetic-TH05",
      "source_location": "Context_design!G16",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A increases at the first sampled time point. E1 states that tissue injury was already present when sampling began; no pre-injury specimens were collected."
    }
  ],
  "context_fields": {
    "response_times": "A increases at the first sampled time point",
    "injury_definition": null,
    "injury_onset": "injury already present when sampling began",
    "temporal_excerpt": "No pre-injury specimens were collected.",
    "time_resolution": null
  },
  "missing_fields": [
    "injury_definition",
    "time_resolution"
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

**Missing-information variant:** Remove all information establishing response_times from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 16, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/DESeq2.html)
- [Background source 2](https://reactome.org/dev/content-service)
