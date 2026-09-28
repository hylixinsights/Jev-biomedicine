# Select questions for a biomedical project

Catalogue 1.1.0 contains 65 proposed questions across 13 domains. Begin with the intended decision and unit of analysis, then select the relevant question cards for their exact wording, answer options, evidence requirements and routing rules. The examples below connect the five scales in Figure 2 with practical research goals. They do not claim validated Jev/Laya performance.

## Population and epidemiology

| Project goal | Catalogue questions | Evidence to prepare |
|---|---|---|
| Classify surveillance cases from clinical free text | [EC06](domains/epidemiology.md#ec06), with EC01/EC04/EC05 for timing, subject and documented status | Protocol case definition, original passages, onset dates and laboratory evidence |
| Prioritize possible outbreak signals | [EC07](domains/epidemiology.md#ec07) | Deduplicated reports, location/time window, baseline and alert criteria |
| Interpret excess cases on maps | [EC08](domains/epidemiology.md#ec08) | Computed clusters, denominators, testing, positivity and ascertainment changes |
| Review SEIR or SEIRS assumptions | [EC09](domains/epidemiology.md#ec09) | Population, simulation horizon, immunity assumptions and relevant evidence |
| Investigate departures from epidemic forecasts | [EC10](domains/epidemiology.md#ec10) | Forecast intervals, observed counts, reporting-delay summaries and backfill |
| Extract exposure information for forecasting | [EC11](domains/epidemiology.md#ec11) | Exposure definition, event date, report availability and forecast cutoff |

Case extraction and signal assessment can supply inputs to outbreak forecasting. Spatial statistics, model fitting, parameter estimation and forecast evaluation remain separate quantitative operations. A suspected-outbreak label is not an outbreak probability. Preserve information-availability dates to avoid using future reports in retrospective forecasts.

## Individual and precision medicine

| Project goal | Catalogue questions | Evidence to prepare |
|---|---|---|
| Prioritize candidate genes for a patient | [GV02](domains/genomics-variants.md#gv02), [GV05](domains/genomics-variants.md#gv05) | Phenotype narrative, age, assessed absent features and gene–disease descriptions |
| Check a candidate variant mechanism | [GV01](domains/genomics-variants.md#gv01), GV03/GV04 for assay relevance | Variant annotation, inheritance, mechanism and transcript evidence |
| Prioritize treatment-related evidence | [PM01](domains/precision-medicine.md#pm01) | Variant, tumor type, therapy, line of treatment and versioned evidence |
| Check whether a clinical or laboratory feature is available in time | [PM02](domains/precision-medicine.md#pm02) | Prediction time, collection time, result-release time and actual clinical workflow |
| Assess a feature’s clinical complementarity | [PM03](domains/precision-medicine.md#pm03) | Intended use, candidate feature and a fixed selected-feature snapshot |
| Evaluate an alternative explanation for a laboratory abnormality | [PM04](domains/precision-medicine.md#pm04) | Assay/specimen, medication or handling history and mechanism evidence |

Combine phenotype match, mechanism compatibility and relevant evidence as separate criteria in an explicit prioritization policy. Predictive gain and feature redundancy require statistical evaluation. PM02 concerns availability only: separately audit outcome-derived features and leakage between development and evaluation data. PM01 organizes evidence for expert review and does not select a treatment autonomously.

## Tissue and organ

| Project goal | Catalogue questions | Evidence to prepare |
|---|---|---|
| Assess inflammation in H&E sections | [HI05](domains/histology.md#hi05) | Region descriptors, tissue compartment, study criteria and image quality |
| Identify CT or X-ray studies meeting research criteria | [RI01](domains/radiology.md#ri01) | Modality-specific criteria and radiology reports or extracted findings |
| Assess pathogen detection in tissue | [HI06](domains/histology.md#hi06), [MB04](domains/microbiome-metagenomics.md#mb04) for material detected | Assay target, specimen, localization, controls and technical quality |
| Review possible processing artifacts | [HI02](domains/histology.md#hi02) | Morphological descriptions and processing/QC notes |
| Prioritize regions for sampling | [HI03](domains/histology.md#hi03), HI01/HI04 for compartment adequacy | Candidate region and a fixed inventory of selected regions |

Specialist methods or expert reports supply the image findings. No direct pixel interpretation by Jev/Laya is assumed. A compatible imaging pattern does not by itself identify a pathogen; a tissue-assay nondetect does not establish absence. Technical invalidity, contradictory findings and missing evidence require distinct handling.

## Cellular analysis

| Project goal | Catalogue questions | Evidence to prepare |
|---|---|---|
| Review capture of effector memory CD4+ T cells or another target population | [FC01](domains/flow-cytometry.md#fc01) | Included-cell profile, parent gates, panel and protocol population definition |
| Check whether target cells were excluded | [FC02](domains/flow-cytometry.md#fc02) | Excluded-cell profiles and condition-specific marker evidence |
| Investigate apparent marker loss | [FC04](domains/flow-cytometry.md#fc04), FC03 for lineage versus state | Clone/epitope, treatment or digestion, controls and reagent evidence |
| Resolve ambiguous single-cell annotations | [SC01](domains/single-cell-spatial.md#sc01), SC02/SC04 as appropriate | Marker profiles, tissue, condition, reference definitions and processing metadata |

Flow cytometry preprocessing, compensation/unmixing and numerical gating are performed upstream. The semantic questions assess biological compatibility of the resulting summaries. Assess the excluded cells separately before interpreting a gate as recovering the complete target population.

## Molecular analysis

| Project goal | Catalogue questions | Evidence to prepare |
|---|---|---|
| Assess a gene–process hypothesis | [TH01](domains/transcriptomics-mechanisms.md#th01) | Functional experiment, controls, endpoints and computed effects |
| Evaluate relevance to the target biological context | [TH02](domains/transcriptomics-mechanisms.md#th02) | Disease, cell type/state and the specific experimental relationship |
| Connect transcript candidates to protein readouts | [BM04](domains/transcriptomics-biomarkers.md#bm04) | Paired RNA/protein results or clearly scoped unpaired evidence |
| Assess evidence for a metabolic conversion | [ML04](domains/metabolomics-lipidomics.md#ml04) | Specialist metabolite annotation, tracing/enzymatic methods and results |

Ask whether the supplied evidence supports a specified relationship. “Does this gene provide new insight?” has no stable answer boundary, and absence from retrieved literature does not establish novelty.

## Choosing answer options

Use the exact options in the selected catalogue entry. Evidence status, phenotype compatibility, assay validity and temporal availability need different categories. “Insufficient evidence” is a judgment about the supplied material; system abstention means declining to make a judgment. A conflict should not be converted to a negative answer or erased by another missing field. Define category precedence before annotation, and use rule-based decisions when structured fields already settle the question.

## Editorial proposal mapping

The 25 examples discussed during Figure 2 revision map to 13 new questions and 12 existing questions, avoiding duplicate IDs for the same decision. Original wording and options remain authoritative for reused questions; explanatory examples above do not silently revise them.

| Editorial example | Stable catalogue ID | Treatment |
|---|---|---|
| P01–P06 | EC06–EC11 | New entries |
| I01 | GV02, with GV05 | Reuse phenotype compatibility and age-dependent contradiction |
| I02 | GV01 | Reuse mechanism compatibility |
| I03–I06 | PM01–PM04 | New entries; PM02 narrowed to availability so leakage remains separate |
| T01 | HI05 | New H&E inflammation question |
| T02 | RI01 | New radiology question |
| T03 | HI06 | New tissue pathogen-assay question |
| T04–T05 | HI02–HI03 | Reuse artifact assessment and region complementarity |
| C01–C04 | FC01, FC02, FC04, SC01 | Reuse gating and annotation questions |
| M01–M04 | TH01, TH02, BM04, ML04 | Reuse molecular evidence questions |

These proposed workflows require task-specific validation. Definitions remain provisional pending domain review; no model output or clinical result is supplied.
