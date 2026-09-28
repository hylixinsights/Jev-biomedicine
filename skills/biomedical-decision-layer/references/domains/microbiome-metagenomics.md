# Microbiome & metagenomics

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[MB01](#mb01) | [MB02](#mb02) | [MB03](#mb03) | [MB04](#mb04)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="mb01"></a>

## MB01

**Question:** At what taxonomic resolution does the cited experiment support the proposed function?

**Decision unit:** One microbial function claim × detected organism

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Detected strain specifically | Experimental evidence concerns the specifically detected strain. |
| 2 | Species-level evidence only | Evidence supports the species level without detected-strain confirmation. |
| 3 | Broader taxonomic group only | Evidence concerns a broader taxonomic group. |
| 4 | Function not experimentally demonstrated | No experiment demonstrates the proposed function. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Taxonomic/strain outputs + functional-experiment text

**Minimum context:** Detected organism and resolution; tested isolate/strain; experimental function and evidence source.

| Required field | Proposed supplier |
|---|---|
| `detected_taxon` | Provided study metadata or source record; researcher confirms identity and meaning |
| `detection_resolution` | Provided study metadata or source record; researcher confirms identity and meaning |
| `tested_isolate` | Provided study metadata or source record; researcher confirms identity and meaning |
| `function` | Provided study metadata or source record; researcher confirms identity and meaning |
| `experiment_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Assign taxonomic IDs and sequence-based functions; resolve explicit strain identifiers; retrieve the experimental organism description.

**Optional cached LLM extraction:** Once per experiment, extract isolate identity and function tested, without transferring the function to all related strains.

**Reuse key:** taxon/strain × function × experimental source

**Semantic work remaining:** Interprets the organism scope of a study where common names or engineered isolates obscure taxonomic transfer.

**Bypass Jev/Laya:** If IDs and evidence scope are curated, compare them in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** MB: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep taxonomic scope, functional potential, production and viability as separate evidence dimensions.

**Future routing:** Strain → retain specific support; species/broader → mark transfer uncertainty; untested/U → evidence gap.

**Expert follow-up:** Review strain/niche and experiment scope; design culture, source-tracing or host-context tests.

**Interpretation limit:** A species-level taxonomic call cannot establish the presence of a strain-specific function.

**Proposed validation:** Expert organism-scope labels; hold out species/strains and source studies.

### MB01 synthetic example

Input material: Metagenomic taxonomic/strain profile, HUMAnN output, isolate metadata and functional papers.

SYNTHETIC: The sample is assigned to species S without strain resolution. E1 demonstrates the function in one engineered strain of S; other strains are not tested.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "MB01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-mb01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "MB01-E1",
      "source_id": "workbook-synthetic-MB01",
      "source_location": "Context_design!G41",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The sample is assigned to species S without strain resolution. E1 demonstrates the function in one engineered strain of S; other strains are not tested."
    }
  ],
  "context_fields": {
    "detected_taxon": "species S",
    "detection_resolution": "species; no strain resolution",
    "tested_isolate": "one engineered strain of S",
    "function": null,
    "experiment_excerpt": "Other strains not tested."
  },
  "missing_fields": [
    "function"
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

**Missing-information variant:** Remove all information establishing detected_taxon from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 41, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://huttenhower.sph.harvard.edu/humann/)

<a id="mb02"></a>

## MB02

**Question:** Does the experiment demonstrate microbial production of the metabolite or only an association with its abundance?

**Decision unit:** One microbe–metabolite relationship × experiment

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Microbial production demonstrated | A source-specific experiment demonstrates production by the microbe. |
| 2 | Association only | Only abundance association is measured. |
| 3 | Host–microbe contribution unresolved | The design cannot distinguish host from microbial contribution. |
| 4 | Evidence against microbial production | A relevant production test provides evidence against microbial production. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Microbial/metabolite profiles + culture/tracing experiment descriptions

**Minimum context:** Microbe, metabolite and proposed production relation; culture/co-culture/tracer design; host contribution and controls.

| Required field | Proposed supplier |
|---|---|
| `microbe` | Provided study metadata or source record; researcher confirms identity and meaning |
| `metabolite` | Provided study metadata or source record; researcher confirms identity and meaning |
| `proposed_relation` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `experiment_system` | Provided study metadata or source record; researcher confirms identity and meaning |
| `production_readout` | Provided study metadata or source record; researcher confirms identity and meaning |
| `controls` | Provided study metadata or source record; researcher confirms identity and meaning |
| `source_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Compute associations and functional annotations; retrieve experiments that measure production rather than only co-occurrence.

**Optional cached LLM extraction:** Extract producer, substrate, metabolite readout and controls once per experiment.

**Reuse key:** microbe × metabolite × experiment × source version

**Semantic work remaining:** Separates a producer claim from a correlation and recognizes unresolved host contribution.

**Bypass Jev/Laya:** Use curated producer/evidence-type records directly when available.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** MB: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep taxonomic scope, functional potential, production and viability as separate evidence dimensions.

**Future routing:** Production → mechanistic review; association/unresolved → non-production evidence tag; against/U → retain for review.

**Expert follow-up:** Review strain/niche and experiment scope; design culture, source-tracing or host-context tests.

**Interpretation limit:** Gene/pathway presence and abundance correlation do not show metabolite production in vivo.

**Proposed validation:** Expert relation labels; downstream culture or tracer validation.

### MB02 synthetic example

Input material: Metagenomic/metabolomic summaries, culture/tracer results and experimental passages.

SYNTHETIC: Species S and M1 correlate in stool samples. E1 is an observational cohort; there is no culture, labeled substrate or source-specific production experiment.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "MB02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-mb02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "MB02-E1",
      "source_id": "workbook-synthetic-MB02",
      "source_location": "Context_design!G42",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: Species S and M1 correlate in stool samples. E1 is an observational cohort; there is no culture, labeled substrate or source-specific production experiment."
    }
  ],
  "context_fields": {
    "microbe": "species S",
    "metabolite": "M1",
    "proposed_relation": "microbial production of M1",
    "experiment_system": "observational stool cohort",
    "production_readout": {
      "measurement_status": "not_measured"
    },
    "controls": null,
    "source_excerpt": "No culture, labeled substrate or source-specific production experiment."
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

**Missing-information variant:** Remove all information establishing microbe from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 42, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://huttenhower.sph.harvard.edu/humann/)

<a id="mb03"></a>

## MB03

**Question:** Does the study test the proposed microbial mechanism in the host niche relevant to this sample?

**Decision unit:** One host-niche mechanism × supporting study

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Same host niche | The mechanism is tested in the target host niche. |
| 2 | Related host niche only | Testing concerns a related host niche. |
| 3 | Culture or non-host environment only | Testing occurs only in culture or another non-host environment. |
| 4 | Niche applicability conflicts | Relevant niche evidence gives incompatible applicability conclusions. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Specimen metadata + experimental niche descriptions

**Minimum context:** Sample anatomical niche and host; proposed mechanism; experimental host/model, substrate and environmental conditions.

| Required field | Proposed supplier |
|---|---|
| `target_niche` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `host` | Provided study metadata or source record; researcher confirms identity and meaning |
| `mechanism` | Provided study metadata or source record; researcher confirms identity and meaning |
| `study_niche` | Provided study metadata or source record; researcher confirms identity and meaning |
| `experimental_conditions` | Provided study metadata or source record; researcher confirms identity and meaning |
| `source_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Normalize explicit anatomical/host IDs; retrieve passages describing where and under what conditions the mechanism was tested.

**Optional cached LLM extraction:** Once per study, extract niche, host and environmental constraints; do not assume culture behavior transfers to the host.

**Reuse key:** microbe/function × niche × experiment version

**Semantic work remaining:** Interprets environment descriptions that determine the scope of mechanistic transfer.

**Bypass Jev/Laya:** If niche/environment compatibility is predefined and fully structured, use code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** MB: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep taxonomic scope, functional potential, production and viability as separate evidence dimensions.

**Future routing:** Same → retain niche support; related/culture → context gap; conflicting/U → specialist review.

**Expert follow-up:** Review strain/niche and experiment scope; design culture, source-tracing or host-context tests.

**Interpretation limit:** Niche compatibility alone does not establish colonization, function or a host effect.

**Proposed validation:** Experts label niche applicability; validate across independent experimental systems.

### MB03 synthetic example

Input material: Sample metadata, taxon/function tables and host/culture experimental methods.

SYNTHETIC: The hypothesis concerns a mucosa-associated microbe. E1 measures the proposed effect in oxygenated rich broth; mucosal substrate and oxygen conditions were not modeled.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "MB03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-mb03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "MB03-E1",
      "source_id": "workbook-synthetic-MB03",
      "source_location": "Context_design!G43",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The hypothesis concerns a mucosa-associated microbe. E1 measures the proposed effect in oxygenated rich broth; mucosal substrate and oxygen conditions were not modeled."
    }
  ],
  "context_fields": {
    "target_niche": "host mucosa",
    "host": null,
    "mechanism": null,
    "study_niche": "oxygenated rich broth",
    "experimental_conditions": "culture without modeled mucosal substrate or oxygen conditions",
    "source_excerpt": "Proposed effect measured in rich broth."
  },
  "missing_fields": [
    "host",
    "mechanism"
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

**Missing-information variant:** Remove all information establishing target_niche from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 43, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://huttenhower.sph.harvard.edu/humann/)

<a id="mb04"></a>

## MB04

**Question:** What biological material does the detection evidence actually establish?

**Decision unit:** One microbial detection result × viability/presence claim

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Culturable organism detected | Culture demonstrates a culturable organism in the tested material. |
| 2 | Viability-associated measurement | A viability-associated assay is reported without culture confirmation. |
| 3 | Nucleic-acid signal only | Detection establishes nucleic acid without viability evidence. |
| 4 | Several evidence types | The record includes multiple listed detection evidence types. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Detection protocol + microbial feature/QC summaries

**Minimum context:** Claim being made; exact detection method; controls; whether organism, viability marker or only nucleic acid was measured.

| Required field | Proposed supplier |
|---|---|
| `detection_method` | Provided study metadata or source record; researcher confirms identity and meaning |
| `measured_material` | Provided study metadata or source record; researcher confirms identity and meaning |
| `control_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `proposed_claim` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `protocol_excerpt` | Source retrieval; optional cached factual extraction; human source check |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Run contaminant/negative-control analyses and read/count thresholds first; retrieve unstructured detection-method descriptions.

**Optional cached LLM extraction:** Extract assay material and controls once per method, without converting a positive molecular result into active infection.

**Reuse key:** detection protocol × evidence type × study version

**Semantic work remaining:** Resolves what a positive 'detection' statement means biologically when assay names are omitted or vague.

**Bypass Jev/Laya:** If assay/evidence class is explicit, map it to material type with code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** MB: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Keep taxonomic scope, functional potential, production and viability as separate evidence dimensions.

**Future routing:** Culture/viability → retain measurement scope; nucleic-acid-only → no viability claim; several/U → inspect evidence separately.

**Expert follow-up:** Review strain/niche and experiment scope; design culture, source-tracing or host-context tests.

**Interpretation limit:** Culturability is condition-dependent; nucleic acid may reflect dead organisms or contamination. This is not an infection diagnosis.

**Proposed validation:** Microbiology-expert labels; evaluate contamination and viability separately.

### MB04 synthetic example

Input material: Sequencing/PCR, culture or viability-assay output, negative controls and methods.

SYNTHETIC: A tissue dataset reports microbial DNA reads and calls the organism an active colonizer. The study contains no culture, replication or viability measurement.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "MB04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-mb04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "MB04-E1",
      "source_id": "workbook-synthetic-MB04",
      "source_location": "Context_design!G44",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: A tissue dataset reports microbial DNA reads and calls the organism an active colonizer. The study contains no culture, replication or viability measurement."
    }
  ],
  "context_fields": {
    "detection_method": "DNA sequencing",
    "measured_material": "microbial DNA reads",
    "control_results": null,
    "proposed_claim": "active colonizer",
    "protocol_excerpt": "No culture, replication or viability measurement."
  },
  "missing_fields": [
    "control_results"
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

**Missing-information variant:** Remove all information establishing detection_method from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 44, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://huttenhower.sph.harvard.edu/humann/)
