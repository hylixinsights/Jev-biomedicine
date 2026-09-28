# Genomics — variants

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[GV01](#gv01) | [GV02](#gv02) | [GV03](#gv03) | [GV04](#gv04) | [GV05](#gv05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="gv01"></a>

## GV01

**Question:** Is the variant's proposed molecular effect compatible with the mechanism described for this gene–disease relationship?

**Decision unit:** One variant × gene–disease mechanism

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Compatible mechanism | The proposed molecular effect agrees with the supplied gene–disease mechanism. |
| 2 | Opposing mechanism | The proposed effect runs opposite to the supplied disease mechanism. |
| 3 | Mechanism-dependent or mixed | Compatibility varies by mechanism, transcript or disease context. |
| 4 | Disease mechanism unresolved | The supplied disease record explicitly leaves the mechanism unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Variant annotation + mechanism evidence excerpts

**Minimum context:** Genome/transcript version; proposed effect and its evidence level; mechanism description for the specific disease, not just the gene.

| Required field | Proposed supplier |
|---|---|
| `variant_id` | Provided study metadata or source record; researcher confirms identity and meaning |
| `transcript_version` | Provided study metadata or source record; researcher confirms identity and meaning |
| `proposed_effect` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `effect_evidence_type` | Provided study metadata or source record; researcher confirms identity and meaning |
| `disease` | Provided study metadata or source record; researcher confirms identity and meaning |
| `mechanism_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Normalize the allele/transcript; retrieve consequence and mechanism annotations; keep predicted and experimentally observed effects distinct.

**Optional cached LLM extraction:** Once per gene–disease source, extract the mechanism statement and exceptions with source spans; do not classify pathogenicity.

**Reuse key:** gene × disease × transcript × mechanism-source version

**Semantic work remaining:** Matches a variant-effect description to disease-specific mechanism wording, rather than treating every damaging variant alike.

**Bypass Jev/Laya:** If both molecular effect and disease mechanism are normalized, use a curated compatibility rule in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** GV: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine evidence-applicability tags for research ranking only; preserve the existing professional variant-classification workflow.

**Future routing:** Compatible → research-review queue; opposing/mixed/unresolved/U → mechanism review before prioritization.

**Expert follow-up:** A genetics expert reviews full phenotype, segregation and functional evidence using the applicable classification framework.

**Interpretation limit:** Compatibility is not an ACMG/AMP classification and does not establish clinical causation.

**Proposed validation:** Genetics experts annotate compatibility; stratify loss/gain/dominant-negative and disease-specific mechanisms.

### GV01 synthetic example

Input material: VCF, VEP output, transcript annotations, curated gene–disease records and functional-study passages.

SYNTHETIC: Variant V is predicted to truncate transcript T. The supplied disease record describes a narrowly defined activating mechanism; another disorder in the same gene is linked to reduced function.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "GV01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-gv01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "GV01-E1",
      "source_id": "workbook-synthetic-GV01",
      "source_location": "Context_design!G17",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Variant V is predicted to truncate transcript T. The supplied disease record describes a narrowly defined activating mechanism; another disorder in the same gene is linked to reduced function."
    }
  ],
  "context_fields": {
    "variant_id": "V",
    "transcript_version": null,
    "proposed_effect": "predicted truncation of transcript T",
    "effect_evidence_type": "prediction",
    "disease": null,
    "mechanism_excerpt": "The target disease record describes a narrowly defined activating mechanism; reduced function is linked to another disorder."
  },
  "missing_fields": [
    "transcript_version",
    "disease"
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

**Missing-information variant:** Remove all information establishing variant_id from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 17, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)
- [Background source 2](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)

<a id="gv02"></a>

## GV02

**Question:** How does the phenotype narrative relate to the supplied descriptions of this gene–disease relationship?

**Decision unit:** One candidate gene–disease relationship × phenotype passage

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Core phenotype match | The assessed phenotype matches defining features and course of the supplied relationship. |
| 2 | Nonspecific overlap only | Overlap concerns common features without a specific pattern match. |
| 3 | Explicit phenotypic contradiction | An adequately assessed phenotype explicitly contradicts the supplied pattern. |
| 4 | Mixed match and contradiction | Some assessed features match while others contradict the pattern. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** De-identified phenotype text + HPO summary + case descriptions

**Minimum context:** Observed and explicitly absent features; age/onset; disease-specific features and exceptions in the evidence.

| Required field | Proposed supplier |
|---|---|
| `observed_features` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assessed_absent_features` | Provided study metadata or source record; researcher confirms identity and meaning |
| `age` | Provided study metadata or source record; researcher confirms identity and meaning |
| `onset` | Provided study metadata or source record; researcher confirms identity and meaning |
| `phenotype_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `reference_cases` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Run phenotype ontology matching first; retain unresolved narrative details, explicit negatives and age-related qualifiers.

**Optional cached LLM extraction:** Optionally extract source case features once per paper. Do not write a diagnosis or convert unmentioned features into negatives.

**Reuse key:** gene–disease × case-description library; patient text remains access-controlled

**Semantic work remaining:** Resolves clinical meaning and disease course that a broad ontology term can conceal.

**Bypass Jev/Laya:** Use ontology/rule matching when structured features fully resolve the comparison.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** GV: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine evidence-applicability tags for research ranking only; preserve the existing professional variant-classification workflow.

**Future routing:** Core → retain phenotype support; nonspecific → no strong support; contradiction/mixed/U → expert review.

**Expert follow-up:** A genetics expert reviews full phenotype, segregation and functional evidence using the applicable classification framework.

**Interpretation limit:** No diagnosis or variant exclusion should be based solely on a text-triage answer.

**Proposed validation:** Expert labels on held-out families; preserve age-dependent and incompletely assessed phenotypes.

### GV02 synthetic example

Input material: Clinical/research phenotype narratives, HPO annotations and supporting case-report passages.

SYNTHETIC: The record describes episodic weakness after exertion. The source describes a progressive congenital structural myopathy. Both share the term 'muscle weakness', but course and trigger differ.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "GV02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-gv02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "GV02-E1",
      "source_id": "workbook-synthetic-GV02",
      "source_location": "Context_design!G18",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The record describes episodic weakness after exertion. The source describes a progressive congenital structural myopathy. Both share the term 'muscle weakness', but course and trigger differ."
    }
  ],
  "context_fields": {
    "observed_features": [
      "episodic weakness after exertion"
    ],
    "assessed_absent_features": null,
    "age": null,
    "onset": null,
    "phenotype_excerpt": "Observed episodic exertional weakness contrasts with progressive congenital structural myopathy.",
    "reference_cases": "Supplied source description only; individual reference cases not provided."
  },
  "missing_fields": [
    "assessed_absent_features",
    "age",
    "onset"
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

**Missing-information variant:** Remove all information establishing observed_features from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 18, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)
- [Background source 2](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)

<a id="gv03"></a>

## GV03

**Question:** Does the functional experiment test the mechanism relevant to this variant–disease relationship?

**Decision unit:** One functional experiment × variant–disease claim

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Relevant mechanism tested | The assay directly tests the disease-relevant mechanism for the specified allele. |
| 2 | Generic functional disruption only | The assay shows disruption without testing the disease-specific mechanism. |
| 3 | Different mechanism tested | The assay tests a mechanism different from the proposed disease mechanism. |
| 4 | Mechanism not tested | No functional test of a mechanism is described. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Functional assay methods/results + mechanism definition

**Minimum context:** Disease mechanism; exact tested allele/construct; endpoint meaning; controls and experimental system.

| Required field | Proposed supplier |
|---|---|
| `disease_mechanism` | Provided study metadata or source record; researcher confirms identity and meaning |
| `tested_allele` | Provided study metadata or source record; researcher confirms identity and meaning |
| `construct` | Provided study metadata or source record; researcher confirms identity and meaning |
| `readout_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `controls` | Provided study metadata or source record; researcher confirms identity and meaning |
| `results_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Match allele/construct identifiers and compute assay effect summaries; retrieve endpoint and control descriptions.

**Optional cached LLM extraction:** Extract intervention, readout and controls once per experiment; keep assay authors' interpretations separate from observations.

**Reuse key:** assay × allele/construct × source version

**Semantic work remaining:** Determines what biological mechanism an assay actually interrogates rather than equating any functional change with relevant evidence.

**Bypass Jev/Laya:** Apply curated assay-to-mechanism rules directly when available.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** GV: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine evidence-applicability tags for research ranking only; preserve the existing professional variant-classification workflow.

**Future routing:** Relevant → expert evidence review; generic/different/not tested → tag evidence limitation; U → retrieve methods.

**Expert follow-up:** A genetics expert reviews full phenotype, segregation and functional evidence using the applicable classification framework.

**Interpretation limit:** Do not automatically assign clinical functional-evidence strength from this answer.

**Proposed validation:** Expert labels against mechanism-specific functional-evidence guidance; include irrelevant but statistically significant assays.

### GV03 synthetic example

Input material: Functional study, curated assay record, variant identity and disease-specific evidence specification.

SYNTHETIC: The disease hypothesis concerns altered ion selectivity. E1 shows reduced protein abundance in an overexpression system but does not measure channel function or selectivity.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "GV03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-gv03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "GV03-E1",
      "source_id": "workbook-synthetic-GV03",
      "source_location": "Context_design!G19",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The disease hypothesis concerns altered ion selectivity. E1 shows reduced protein abundance in an overexpression system but does not measure channel function or selectivity."
    }
  ],
  "context_fields": {
    "disease_mechanism": "altered ion selectivity",
    "tested_allele": null,
    "construct": null,
    "readout_definition": "protein abundance in an overexpression system",
    "controls": null,
    "results_excerpt": "Reduced abundance; channel function and selectivity not measured."
  },
  "missing_fields": [
    "tested_allele",
    "construct",
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

**Missing-information variant:** Remove all information establishing disease_mechanism from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 19, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)
- [Background source 2](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)

<a id="gv04"></a>

## GV04

**Question:** Does the assay system represent the transcript or isoform implicated by the proposed variant mechanism?

**Decision unit:** One functional assay × transcript/isoform hypothesis

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Relevant endogenous transcript | The assay represents the relevant endogenous transcript or isoform. |
| 2 | Relevant sequence in an artificial construct | The relevant sequence is present in an artificial assay construct. |
| 3 | Different transcript or isoform | The tested transcript or isoform differs from the proposed target. |
| 4 | Transcript representation unresolved | Available assay documentation explicitly leaves transcript representation unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Transcript annotations + assay construct/method descriptions

**Minimum context:** Disease-relevant transcript definition; exon/isoform context; tested cell type and construct design.

| Required field | Proposed supplier |
|---|---|
| `target_transcript` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `exon_context` | Provided study metadata or source record; researcher confirms identity and meaning |
| `construct_mapping` | Existing statistical or specialist output; record tool, version, units and QC |
| `assay_system` | Provided study metadata or source record; researcher confirms identity and meaning |
| `endogenous_expression` | Provided study metadata or source record; researcher confirms identity and meaning |
| `methods_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compare sequence/exon identities in code; retrieve descriptions of promoter, splicing context and endogenous versus minigene expression.

**Optional cached LLM extraction:** Extract construct design and cell-system details once per assay when not machine-readable.

**Reuse key:** transcript/construct × assay × reference release

**Semantic work remaining:** Interprets whether the experimental design contains the biological process required to test the mechanism.

**Bypass Jev/Laya:** Sequence identity and exon inclusion checks belong in code; bypass once system suitability is curated.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** GV: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine evidence-applicability tags for research ranking only; preserve the existing professional variant-classification workflow.

**Future routing:** Endogenous → retain context evidence; artificial → note scope; different/unresolved/U → assay-design review.

**Expert follow-up:** A genetics expert reviews full phenotype, segregation and functional evidence using the applicable classification framework.

**Interpretation limit:** Matching sequence does not by itself validate the assay or establish a splice effect.

**Proposed validation:** Experts label assay representation; test minigene, cDNA and endogenous-transcript cases.

### GV04 synthetic example

Input material: Transcript annotation, exon expression, construct sequence/metadata and assay methods.

SYNTHETIC: The variant is in an exon included in the disease-relevant isoform. The assay uses a cDNA construct lacking introns; the proposed mechanism is altered splicing of that exon.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "GV04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-gv04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "GV04-E1",
      "source_id": "workbook-synthetic-GV04",
      "source_location": "Context_design!G20",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The variant is in an exon included in the disease-relevant isoform. The assay uses a cDNA construct lacking introns; the proposed mechanism is altered splicing of that exon."
    }
  ],
  "context_fields": {
    "target_transcript": null,
    "exon_context": "variant exon included in disease-relevant isoform",
    "construct_mapping": "cDNA construct lacks introns",
    "assay_system": "cDNA construct",
    "endogenous_expression": null,
    "methods_excerpt": "Proposed mechanism is altered exon splicing; assay construct lacks introns."
  },
  "missing_fields": [
    "target_transcript",
    "endogenous_expression"
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

**Missing-information variant:** Remove all information establishing target_transcript from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 20, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)
- [Background source 2](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)

<a id="gv05"></a>

## GV05

**Question:** Does the reported absence constitute a meaningful contradiction at the participant's age and assessment stage?

**Decision unit:** One reported absent phenotype × disease hypothesis

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Meaningful contradiction | An adequately assessed absence contradicts expectations at the participant age and stage. |
| 2 | Absence expected or permissible at this stage | Natural history permits the absence at that age or stage. |
| 3 | Feature not adequately assessed | The record does not adequately assess the feature, even if absence is reported. |
| 4 | Conflicting natural-history evidence | Relevant natural-history sources disagree on when the feature should be present. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Phenotype assessment text + age/onset + natural-history evidence

**Minimum context:** One explicitly queried feature; what was assessed; participant age; source description of onset/penetrance or exceptions.

| Required field | Proposed supplier |
|---|---|
| `feature` | Provided study metadata or source record; researcher confirms identity and meaning |
| `assessment_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `participant_age` | Provided study metadata or source record; researcher confirms identity and meaning |
| `natural_history_excerpt` | Source retrieval; optional cached factual extraction; human source check |
| `penetrance_qualification` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Identify explicit negatives versus missing fields; align ages and assessment dates; retrieve the relevant natural-history passage.

**Optional cached LLM extraction:** Optionally extract onset and assessment qualifications from source documents; do not assume complete penetrance.

**Reuse key:** feature × disease × natural-history version; assessment context per participant

**Semantic work remaining:** Interprets an absence in relation to age and measurement adequacy rather than treating a missing HPO term as exclusionary.

**Bypass Jev/Laya:** Use code when age thresholds, assessment validity and penetrance rules are fully specified.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** GV: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Combine evidence-applicability tags for research ranking only; preserve the existing professional variant-classification workflow.

**Future routing:** Contradiction → expert review; permissible → do not penalize; unassessed/conflicting/U → preserve uncertainty.

**Expert follow-up:** A genetics expert reviews full phenotype, segregation and functional evidence using the applicable classification framework.

**Interpretation limit:** No absence-based automatic exclusion; disease expression and assessment can be incomplete.

**Proposed validation:** Adjudicated age/assessment-sensitive cases; hold out families and reference documents.

### GV05 synthetic example

Input material: Phenotype record, assessment methods and disease natural-history passages.

SYNTHETIC: A childhood participant has no recorded hearing impairment. The source says this feature generally emerges in adulthood; the record does not include formal audiometry.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "GV05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-gv05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "GV05-E1",
      "source_id": "workbook-synthetic-GV05",
      "source_location": "Context_design!G21",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A childhood participant has no recorded hearing impairment. The source says this feature generally emerges in adulthood; the record does not include formal audiometry."
    }
  ],
  "context_fields": {
    "feature": "hearing impairment",
    "assessment_excerpt": "No recorded hearing impairment; no formal audiometry in record.",
    "participant_age": "childhood; exact age not supplied",
    "natural_history_excerpt": "Feature generally emerges in adulthood.",
    "penetrance_qualification": null
  },
  "missing_fields": [
    "penetrance_qualification"
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

**Missing-information variant:** Remove all information establishing feature from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 21, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://jun2026.archive.ensembl.org/info/docs/tools/vep/index.html)
- [Background source 2](https://clinicalgenome.org/tools/clingen-variant-classification-guidance/)
