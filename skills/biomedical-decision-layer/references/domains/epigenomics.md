# Epigenomics

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[EP01](#ep01) | [EP02](#ep02) | [EP03](#ep03) | [EP04](#ep04)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="ep01"></a>

## EP01

**Question:** Does the experiment support this regulatory element–gene link in the relevant cell state?

**Decision unit:** One element–gene link × supporting experiment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Support in target state | A direct experiment supports the link in the specified cell state. |
| 2 | Support in another state only | Experimental support concerns a different cell state. |
| 3 | Contradictory target-state evidence | A target-state experiment contradicts the proposed link. |
| 4 | Link not directly tested | The element–gene link is not directly tested. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Regulatory-link statistics + cell-state metadata + experiment text

**Minimum context:** Element/gene IDs; proposed state; link evidence type; perturbation, contact or expression experiment and controls.

| Required field | Proposed supplier |
|---|---|
| `element_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `gene_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_state` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `computed_link` | Existing statistical or specialist output; record tool, version, units and QC |
| `evidence_type` | Provided study metadata or source record; researcher confirms identity and meaning |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Call peaks and compute links/contact statistics; normalize coordinates; retrieve state-specific experiments.

**Optional cached LLM extraction:** Extract the tested element, readout gene and biological state once per experiment; do not label the link as causal.

**Reuse key:** element × gene × state × reference genome × experiment

**Semantic work remaining:** Interprets state and experimental applicability that may be hidden in free-text model descriptions.

**Bypass Jev/Laya:** Use code when state identity and evidence classes are already curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EP: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine locus/state/assay-scope tags without converting association evidence to causal evidence.

**Future routing:** Target → functional-review queue; other/not tested → context gap; contradictory/U → retain for review.

**Expert follow-up:** Review locus-specific methods, controls and alternate targets; design state-matched regulatory perturbations.

**Interpretation limit:** Co-accessibility or chromatin contact alone does not prove regulation of the gene.

**Proposed validation:** Experts label state and experiment applicability; split by locus/study.

### EP01 synthetic example

Input material: ATAC/ChIP data, chromatin-contact/link outputs, cCRE annotations and functional-study passages.

SYNTHETIC: Element E is linked to gene A in an activated epithelial state. The cited deletion experiment reduces A only in a resting transformed cell line; target-state cells were not tested.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EP01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ep01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EP01-E1",
      "source_id": "workbook-synthetic-EP01",
      "source_location": "Context_design!G27",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Element E is linked to gene A in an activated epithelial state. The cited deletion experiment reduces A only in a resting transformed cell line; target-state cells were not tested."
    }
  ],
  "context_fields": {
    "element_id": "E",
    "gene_id": "A",
    "target_state": "activated epithelial state",
    "computed_link": null,
    "evidence_type": "element deletion experiment",
    "experiment_excerpt": "Deletion reduces A only in a resting transformed cell line; target-state cells not tested."
  },
  "missing_fields": [
    "computed_link"
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

**Missing-information variant:** Remove all information establishing element_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 27, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://screen.encodeproject.org/)

<a id="ep02"></a>

## EP02

**Question:** Does the experiment separate a regulatory effect from a change in cell identity?

**Decision unit:** One regulatory perturbation × observed expression change

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Effect measured within preserved identity | The regulatory effect is measured while relevant cell identity is preserved. |
| 2 | Cell identity also changes | The intervention also changes cell identity, limiting attribution. |
| 3 | Identity was not assessed | Identity was not assessed in the reported experiment. |
| 4 | No perturbation experiment | No regulatory perturbation experiment is supplied. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Perturbation readouts + cell-identity controls + methods

**Minimum context:** Element manipulation; timing; target-gene result; lineage/state measurements and composition controls.

| Required field | Proposed supplier |
|---|---|
| `perturbation` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_effect` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `identity_controls` | Provided study metadata or source record; researcher confirms identity and meaning |
| `composition_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `timepoints` | Provided study metadata or source record; researcher confirms identity and meaning |
| `source_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute target-gene effects and identity/composition summaries; preserve the timing of each measurement.

**Optional cached LLM extraction:** Extract which identity controls were measured and when, without deciding whether the change is causal.

**Reuse key:** perturbation × cell system × time point × source version

**Semantic work remaining:** Interprets control design to distinguish a within-cell regulatory observation from a shifted cellular population.

**Bypass Jev/Laya:** Use code if preserved-identity criteria and measurements are predefined and sufficient.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EP: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine locus/state/assay-scope tags without converting association evidence to causal evidence.

**Future routing:** Preserved → retain within-identity evidence; identity changes/unassessed → alternative interpretation; no perturbation/U → evidence gap.

**Expert follow-up:** Review locus-specific methods, controls and alternate targets; design state-matched regulatory perturbations.

**Interpretation limit:** Preserved identity reduces one alternative explanation but does not establish direct regulation.

**Proposed validation:** Expert control-design labels, including timing and lineage shifts.

### EP02 synthetic example

Input material: Perturbation expression data, identity-marker measurements, cell-composition results and methods.

SYNTHETIC: Deleting E lowers gene A after cells lose their original lineage markers and acquire a different fate. The study has no early measurement before the identity change.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EP02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ep02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EP02-E1",
      "source_id": "workbook-synthetic-EP02",
      "source_location": "Context_design!G28",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Deleting E lowers gene A after cells lose their original lineage markers and acquire a different fate. The study has no early measurement before the identity change."
    }
  ],
  "context_fields": {
    "perturbation": "deletion of E",
    "target_effect": "lower gene A",
    "identity_controls": "original lineage markers lost; different fate acquired",
    "composition_results": null,
    "timepoints": "No early measurement before identity change.",
    "source_excerpt": "A decreases after the cells acquire a different fate."
  },
  "missing_fields": [
    "composition_results"
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

**Missing-information variant:** Remove all information establishing perturbation from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 28, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://screen.encodeproject.org/)

<a id="ep03"></a>

## EP03

**Question:** Does the assay test the element in its endogenous regulatory context?

**Decision unit:** One enhancer assay × endogenous-regulation claim

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Endogenous locus perturbed | The element is perturbed at its endogenous genomic locus. |
| 2 | Reporter or ectopic construct only | Only a reporter or ectopic construct is tested. |
| 3 | Both endogenous and reporter evidence | Both endogenous-locus and reporter experiments are supplied. |
| 4 | Endogenous context unresolved | The described assay does not resolve its endogenous context. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Assay construct/design descriptions + locus metadata

**Minimum context:** Claimed endogenous relationship; physical location of tested sequence; reporter design or genomic perturbation details.

| Required field | Proposed supplier |
|---|---|
| `element_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `claimed_context` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_design` | Provided study metadata or source record; researcher confirms identity and meaning |
| `genomic_location` | Provided study metadata or source record; researcher confirms identity and meaning |
| `construct` | Provided study metadata or source record; researcher confirms identity and meaning |
| `methods_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Match genomic coordinates and construct IDs; extract reported assay classes and effect summaries.

**Optional cached LLM extraction:** Once per protocol, extract whether the sequence is in a reporter, ectopic integration or its native locus.

**Reuse key:** assay protocol × construct/locus × source version

**Semantic work remaining:** Interprets methods wording to avoid confusing reporter activity with a native gene-regulatory test.

**Bypass Jev/Laya:** Use code when assay design and locus context are explicit structured fields.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EP: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine locus/state/assay-scope tags without converting association evidence to causal evidence.

**Future routing:** Endogenous/both → retain native-context evidence; reporter → tag transfer gap; unresolved/U → methods review.

**Expert follow-up:** Review locus-specific methods, controls and alternate targets; design state-matched regulatory perturbations.

**Interpretation limit:** Reporter activity does not by itself identify the endogenous target gene.

**Proposed validation:** Experts label assay context; include episomal and integrated constructs.

### EP03 synthetic example

Input material: MPRA/reporter results, CRISPR experiment metadata and methods.

SYNTHETIC: Sequence E increases luciferase expression in a plasmid. The paper calls E an enhancer of nearby gene A, but no manipulation of the endogenous locus is reported.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EP03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ep03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EP03-E1",
      "source_id": "workbook-synthetic-EP03",
      "source_location": "Context_design!G29",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Sequence E increases luciferase expression in a plasmid. The paper calls E an enhancer of nearby gene A, but no manipulation of the endogenous locus is reported."
    }
  ],
  "context_fields": {
    "element_id": "E",
    "claimed_context": "enhancer of nearby gene A",
    "assay_design": "luciferase reporter",
    "genomic_location": null,
    "construct": "plasmid",
    "methods_excerpt": "No manipulation of endogenous locus reported."
  },
  "missing_fields": [
    "genomic_location"
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

**Missing-information variant:** Remove all information establishing element_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 29, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://screen.encodeproject.org/)

<a id="ep04"></a>

## EP04

**Question:** Was methylation directly manipulated in the experiment or only measured as a response?

**Decision unit:** One methylation experiment × proposed regulatory mechanism

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Methylation directly manipulated | Methylation itself is an experimental intervention. |
| 2 | Methylation measured after another intervention | Methylation is measured after a different intervention. |
| 3 | Observational methylation association only | Methylation and expression are observed without an intervention. |
| 4 | Mixed experiment types | The supplied experiment set contains multiple listed design types. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Methylation results + intervention design + endpoint text

**Minimum context:** Proposed locus–gene mechanism; exact intervention; when methylation and expression were measured.

| Required field | Proposed supplier |
|---|---|
| `locus` | Provided study metadata or source record; researcher confirms identity and meaning |
| `proposed_mechanism` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `intervention` | Provided study metadata or source record; researcher confirms identity and meaning |
| `methylation_measurement` | Provided study metadata or source record; researcher confirms identity and meaning |
| `expression_measurement` | Provided study metadata or source record; researcher confirms identity and meaning |
| `design_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute DMRs and associations; identify stated intervention arms; retrieve locus-specific manipulation descriptions.

**Optional cached LLM extraction:** Extract intervention and measurement identities once per experiment. Do not turn an association into a directional mechanism.

**Reuse key:** locus × experiment × intervention × version

**Semantic work remaining:** Separates an experimentally manipulated regulatory variable from a measured correlate when titles overstate mechanism.

**Bypass Jev/Laya:** Use curated study-design labels directly if present.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EP: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine locus/state/assay-scope tags without converting association evidence to causal evidence.

**Future routing:** Direct → mechanistic-review queue; response/observational → non-causal evidence tag; mixed/U → examine designs separately.

**Expert follow-up:** Review locus-specific methods, controls and alternate targets; design state-matched regulatory perturbations.

**Interpretation limit:** Even targeted methylation manipulation requires specificity and off-target controls before causal interpretation.

**Proposed validation:** Experts label intervention versus measured response; include titles that differ from methods.

### EP04 synthetic example

Input material: Methylation arrays/sequencing, expression results and experimental methods.

SYNTHETIC: Treatment T changes both methylation at E and expression of A. The study never manipulates methylation independently of T.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EP04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ep04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EP04-E1",
      "source_id": "workbook-synthetic-EP04",
      "source_location": "Context_design!G30",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Treatment T changes both methylation at E and expression of A. The study never manipulates methylation independently of T."
    }
  ],
  "context_fields": {
    "locus": "E",
    "proposed_mechanism": "methylation-mediated regulation of A",
    "intervention": "treatment T",
    "methylation_measurement": "changes at E after T",
    "expression_measurement": "A changes after T",
    "design_excerpt": "Methylation was not manipulated independently of T."
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

**Missing-information variant:** Remove all information establishing locus from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 30, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://screen.encodeproject.org/)
