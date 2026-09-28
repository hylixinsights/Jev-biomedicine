# Proteomics

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[PR01](#pr01) | [PR02](#pr02) | [PR03](#pr03) | [PR04](#pr04) | [PR05](#pr05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="pr01"></a>

## PR01

**Question:** Do the assays measure the same biological entity?

**Decision unit:** Two assays reporting discordant protein results

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Same molecular species | Both assays target the same molecular form. |
| 2 | Different forms or epitopes | Assays target distinct forms, cleavage products or epitopes. |
| 3 | Partly overlapping target scope | Target scopes overlap but neither is identical nor wholly separate. |
| 4 | Target specificity unresolved | Available specificity evidence does not establish the molecular scopes. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Assay/peptide annotations + epitope/proteoform descriptions

**Minimum context:** Protein IDs; epitopes or quantified peptides; isoforms, cleavage products, complexes and sample preparation for both assays.

| Required field | Proposed supplier |
|---|---|
| `assay_A` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_B` | Provided study metadata or source record; researcher confirms identity and meaning |
| `target_ids` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `peptide_epitope_map` | Provided study metadata or source record; researcher confirms identity and meaning |
| `molecular_forms` | Provided study metadata or source record; researcher confirms identity and meaning |
| `preparation` | Provided study metadata or source record; researcher confirms identity and meaning |
| `scope_excerpts` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Map peptides and target accessions, compare overlap, quantify discordance and retrieve target-scope passages.

**Optional cached LLM extraction:** Once per assay document, extract recognition of full-length, cleaved, bound or modified material; preserve unresolved specificity.

**Reuse key:** assay pair × target form × document/reference version

**Semantic work remaining:** Interprets assay scope that is not captured by the shared gene/protein name.

**Bypass Jev/Laya:** Use code if molecular-species and epitope mappings already resolve overlap.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** PR: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Stratify evidence by molecular species and assay scope before combining quantitative results.

**Future routing:** Same → investigate technical/biological discordance; different/partial → analyze entities separately; unresolved/U → assay review.

**Expert follow-up:** Review assay specificity, proteoforms and functional readouts; plan orthogonal biochemical validation.

**Interpretation limit:** Discordant results need not be failed replication when assays measure different molecular entities.

**Proposed validation:** Proteomics/assay experts label target equivalence; verify peptide and epitope mappings.

### PR01 synthetic example

Input material: Proteomics feature table, peptide–protein mapping, antibody/aptamer documentation and validation methods.

SYNTHETIC: Assay A recognizes an N-terminal cleavage fragment. Assay B quantifies a peptide retained only in full-length protein A. Their group effects point in opposite directions.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "PR01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-pr01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "PR01-E1",
      "source_id": "workbook-synthetic-PR01",
      "source_location": "Context_design!G31",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Assay A recognizes an N-terminal cleavage fragment. Assay B quantifies a peptide retained only in full-length protein A. Their group effects point in opposite directions."
    }
  ],
  "context_fields": {
    "assay_A": "recognizes N-terminal cleavage fragment",
    "assay_B": "quantifies peptide retained only in full-length A",
    "target_ids": [
      "A"
    ],
    "peptide_epitope_map": "N-terminal fragment versus full-length-specific peptide",
    "molecular_forms": [
      "cleavage fragment",
      "full-length protein"
    ],
    "preparation": null,
    "scope_excerpts": "Group effects point in opposite directions."
  },
  "missing_fields": [
    "preparation"
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

**Missing-information variant:** Remove all information establishing assay_A from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 31, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/MSstats.html)
- [Background source 2](https://pmc.ncbi.nlm.nih.gov/articles/PMC10335836/)

<a id="pr02"></a>

## PR02

**Question:** Does the experiment support altered protein activity or only altered abundance?

**Decision unit:** One protein claim × supporting experiment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Activity change measured | A functional or catalytic assay measures the claimed activity change. |
| 2 | Abundance change only | Only abundance is measured. |
| 3 | Both activity and abundance measured | Both activity and abundance are measured. |
| 4 | Functional results conflict | Comparable functional measurements support incompatible activity interpretations. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Protein quantification + functional assay description

**Minimum context:** Claimed activity; actual endpoint definition; abundance, activity and modification measurements with controls.

| Required field | Proposed supplier |
|---|---|
| `protein_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `claimed_activity` | Provided study metadata or source record; researcher confirms identity and meaning |
| `abundance_result` | Existing statistical or specialist output; record tool, version, units and QC |
| `activity_result` | Existing statistical or specialist output; record tool, version, units and QC |
| `assay_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `controls` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute abundance/activity changes independently; retrieve what the activity readout measures and how it was normalized.

**Optional cached LLM extraction:** Extract assay readout and normalization once per experiment; do not equate abundance or phosphorylation with activation.

**Reuse key:** protein × function × experimental assay × source version

**Semantic work remaining:** Distinguishes a molecular quantity from the functional claim attached to it.

**Bypass Jev/Laya:** Use code when assay endpoints are already curated as activity versus abundance.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** PR: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Stratify evidence by molecular species and assay scope before combining quantitative results.

**Future routing:** Activity/both → retain functional evidence; abundance → abundance-only tag; conflict/U → review.

**Expert follow-up:** Review assay specificity, proteoforms and functional readouts; plan orthogonal biochemical validation.

**Interpretation limit:** Abundance does not determine activity; activity measurements can themselves be indirect.

**Proposed validation:** Expert readout-scope labels; evaluate activity and abundance concordance independently.

### PR02 synthetic example

Input material: Protein/peptide measurements, activity assays and experimental methods/results.

SYNTHETIC: Protein A abundance increases. The only validation is an immunoblot for total A; the discussion describes pathway activation without a catalytic or functional assay.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "PR02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-pr02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "PR02-E1",
      "source_id": "workbook-synthetic-PR02",
      "source_location": "Context_design!G32",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Protein A abundance increases. The only validation is an immunoblot for total A; the discussion describes pathway activation without a catalytic or functional assay."
    }
  ],
  "context_fields": {
    "protein_id": "A",
    "claimed_activity": "pathway activation",
    "abundance_result": "increased",
    "activity_result": {
      "measurement_status": "not_measured",
      "basis": "No catalytic or functional assay in supplied evidence."
    },
    "assay_definition": "total-A immunoblot",
    "controls": null
  },
  "missing_fields": [
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

**Missing-information variant:** Remove all information establishing protein_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 32, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/MSstats.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="pr03"></a>

## PR03

**Question:** Do the handling notes match a documented process capable of generating this protein signal?

**Decision unit:** One protein signal × named handling mechanism

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Matches documented mechanism | Recorded handling meets the documented conditions for the named signal-generating mechanism. |
| 2 | Partly matches the mechanism | Only some necessary handling conditions are documented or matched. |
| 3 | Handling contradicts the mechanism | Recorded conditions are inconsistent with the specified mechanism. |
| 4 | Several handling mechanisms remain possible | More than one handling mechanism remains compatible with the facts. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Preanalytical metadata + mechanism evidence excerpts

**Minimum context:** Observed protein form/signal; collection and processing notes; one documented release/degradation/interference mechanism and its conditions.

| Required field | Proposed supplier |
|---|---|
| `protein_form` | Provided study metadata or source record; researcher confirms identity and meaning |
| `signal` | Provided study metadata or source record; researcher confirms identity and meaning |
| `handling_note` | Provided study metadata or source record; researcher confirms identity and meaning |
| `computed_QC` | Existing statistical or specialist output; record tool, version, units and QC |
| `mechanism_conditions` | Source retrieval; optional cached factual extraction; human source check |
| `mechanism_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute time-to-processing and QC associations; retrieve the mechanism passage and the sample's original handling note.

**Optional cached LLM extraction:** Once per handling study/SOP, extract the condition and effect on the measured form; avoid guessing an unreported mechanism.

**Reuse key:** protein form × processing mechanism × SOP/reference version

**Semantic work remaining:** Matches nonstandard handling descriptions to a documented molecular artifact mechanism.

**Bypass Jev/Laya:** Explicit processing thresholds and established QC-failure rules belong in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** PR: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Stratify evidence by molecular species and assay scope before combining quantitative results.

**Future routing:** Match/partial → technical-sensitivity review; contradiction → no support for this mechanism; several/U → inspect controls.

**Expert follow-up:** Review assay specificity, proteoforms and functional readouts; plan orthogonal biochemical validation.

**Interpretation limit:** Compatibility does not establish that handling caused the observed difference.

**Proposed validation:** Experts label mechanism compatibility; test on controlled preanalytical experiments.

### PR03 synthetic example

Input material: Sample logs, hemolysis/processing QC, assay results and experimental handling studies.

SYNTHETIC: Protein A is elevated in samples held before separation. E1 shows release of the measured form from residual cells during prolonged room-temperature storage under the same collection procedure.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "PR03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-pr03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "PR03-E1",
      "source_id": "workbook-synthetic-PR03",
      "source_location": "Context_design!G33",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Protein A is elevated in samples held before separation. E1 shows release of the measured form from residual cells during prolonged room-temperature storage under the same collection procedure."
    }
  ],
  "context_fields": {
    "protein_form": "form released from residual cells in E1",
    "signal": "elevated A",
    "handling_note": "samples held before separation",
    "computed_QC": null,
    "mechanism_conditions": "prolonged room-temperature storage under the same collection procedure",
    "mechanism_excerpt": "E1 shows release of the measured form from residual cells."
  },
  "missing_fields": [
    "computed_QC"
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

**Missing-information variant:** Remove all information establishing protein_form from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 33, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/MSstats.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="pr04"></a>

## PR04

**Question:** Does the cited experiment test the specific modification implicated in the proposed functional effect?

**Decision unit:** One phosphosite/proteoform × functional interpretation

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Specific modification tested | The experiment tests the specified site or proteoform modification. |
| 2 | Protein-level function only | Evidence concerns protein-level function without a specific modification test. |
| 3 | Different site or modification tested | The test concerns a different site or modification. |
| 4 | Conflicting site-specific evidence | Comparable experiments disagree about the specified modification. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Site-localization results + modification metadata + functional text

**Minimum context:** Site/proteoform identity and localization uncertainty; proposed effect; mutant, enzymatic or binding experiment at the relevant site.

| Required field | Proposed supplier |
|---|---|
| `protein_isoform` | Provided study metadata or source record; researcher confirms identity and meaning |
| `site` | Provided study metadata or source record; researcher confirms identity and meaning |
| `localization_status` | Existing statistical or specialist output; record tool, version, units and QC |
| `proposed_function` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `tested_modification` | Provided study metadata or source record; researcher confirms identity and meaning |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Map residues/isoforms and apply localization/QC rules; compute modified versus total-protein changes; retrieve site-specific experiments.

**Optional cached LLM extraction:** Extract the actual modification/manipulation and functional readout once per study.

**Reuse key:** protein isoform × site × function × evidence version

**Semantic work remaining:** Checks whether the functional interpretation is actually supported at the measured molecular detail.

**Bypass Jev/Laya:** Residue matching and localization thresholds are deterministic; bypass if the experiment/site relation is curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** PR: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Stratify evidence by molecular species and assay scope before combining quantitative results.

**Future routing:** Specific → retain evidence; protein-only/different → do not transfer the functional label; conflicting/U → specialist review.

**Expert follow-up:** Review assay specificity, proteoforms and functional readouts; plan orthogonal biochemical validation.

**Interpretation limit:** Modification abundance or a confident site assignment does not establish an activating/inhibitory role.

**Proposed validation:** Expert site-to-function evidence labels; separate localization errors from semantic errors.

### PR04 synthetic example

Input material: Modified-peptide table, localization scores, protein mapping and source methods/results.

SYNTHETIC: The measured feature maps to site S1. The cited activation experiment changes site S2 and measures total enzyme activity; it contains no test of S1.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "PR04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-pr04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "PR04-E1",
      "source_id": "workbook-synthetic-PR04",
      "source_location": "Context_design!G34",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The measured feature maps to site S1. The cited activation experiment changes site S2 and measures total enzyme activity; it contains no test of S1."
    }
  ],
  "context_fields": {
    "protein_isoform": null,
    "site": "S1",
    "localization_status": null,
    "proposed_function": "activation",
    "tested_modification": "S2",
    "experiment_excerpt": "S2 experiment measures total enzyme activity; no S1 test."
  },
  "missing_fields": [
    "protein_isoform",
    "localization_status"
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

**Missing-information variant:** Remove all information establishing protein_isoform from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 34, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/MSstats.html)
- [Background source 2](https://reactome.org/dev/content-service)

<a id="pr05"></a>

## PR05

**Question:** Does the evidence concern the assembled complex or an isolated subunit?

**Decision unit:** One subunit result × complex-function claim

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Assembled complex measured | The assay measures the assembled complex. |
| 2 | Isolated subunit only | The assay measures an isolated subunit without assembly evidence. |
| 3 | Both complex and subunit measured | Both complex assembly and subunit measurements are available. |
| 4 | Assembly state unresolved | Assay conditions leave assembly state unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Complex annotations + biochemical assay descriptions

**Minimum context:** Claimed complex/function; measured subunit; native assembly or co-purification conditions and readout.

| Required field | Proposed supplier |
|---|---|
| `complex_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `measured_subunit` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assay_conditions` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assembly_readout` | Provided study metadata or source record; researcher confirms identity and meaning |
| `functional_claim` | Provided study metadata or source record; researcher confirms identity and meaning |
| `source_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Map subunits to complex records; retrieve assay conditions and measured components; keep abundance calculations upstream.

**Optional cached LLM extraction:** Extract whether the assay preserves/tests assembly and what was detected once per experiment.

**Reuse key:** complex × assay × source version

**Semantic work remaining:** Interprets whether the experimental evidence measures the biological unit named in the hypothesis.

**Bypass Jev/Laya:** Use curated assay/assembly annotations when available.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** PR: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Stratify evidence by molecular species and assay scope before combining quantitative results.

**Future routing:** Complex/both → retain assembly evidence; isolated → complex-evidence gap; unresolved/U → biochemical review.

**Expert follow-up:** Review assay specificity, proteoforms and functional readouts; plan orthogonal biochemical validation.

**Interpretation limit:** Subunit abundance does not establish assembly, stoichiometry or complex activity.

**Proposed validation:** Biochemistry-expert labels on assay scope; validate assembly/activity experimentally.

### PR05 synthetic example

Input material: Protein-group results, complex membership records, native/denaturing assay methods and functional studies.

SYNTHETIC: Subunit A increases in denaturing proteomics. The hypothesis is increased activity of complex A/B/C, but neither complex assembly nor its activity is measured.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "PR05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-pr05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "PR05-E1",
      "source_id": "workbook-synthetic-PR05",
      "source_location": "Context_design!G35",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Subunit A increases in denaturing proteomics. The hypothesis is increased activity of complex A/B/C, but neither complex assembly nor its activity is measured."
    }
  ],
  "context_fields": {
    "complex_id": "A/B/C",
    "measured_subunit": "A",
    "assay_conditions": "denaturing proteomics",
    "assembly_readout": {
      "measurement_status": "not_measured"
    },
    "functional_claim": "increased complex activity",
    "source_excerpt": "Neither assembly nor complex activity measured."
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

**Missing-information variant:** Remove all information establishing complex_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 35, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://bioconductor.org/packages/release/bioc/html/MSstats.html)
- [Background source 2](https://reactome.org/dev/content-service)
