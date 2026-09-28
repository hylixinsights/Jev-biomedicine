# Precision medicine

Proposed research tasks, introduced in catalogue 1.1.0. No model execution or biomedical validation has been performed.

[PM01](#pm01) | [PM02](#pm02) | [PM03](#pm03) | [PM04](#pm04)

<a id="pm01"></a>

## PM01

**Question:** How does the supplied evidence relate variant [X] to response to therapy [Y] in the patient’s tumor context?

**Decision unit:** One variant × therapy × patient tumor context

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supports sensitivity only | Applicable evidence supports sensitivity and no applicable resistance evidence is present in the defined review scope. |
| 2 | Supports resistance only | Applicable evidence supports resistance and no applicable sensitivity evidence is present in the defined review scope. |
| 3 | Contains both sensitivity and resistance evidence | Applicable sensitivity and resistance evidence are both present; differences in context require review. |
| 4 | No applicable response evidence in the reviewed material | A sufficiently complete, defined review found no applicable sensitivity or resistance evidence for this variant–therapy–tumor context. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Exact alteration, tumor type, treatment, line of therapy, relevant studies or curated knowledgebase entries and their versions. “No applicable evidence” requires an adequate, defined review scope; an incomplete dossier is insufficient evidence.

**Minimum context:** Exact alteration, tumor type, treatment, line of therapy, relevant studies or curated knowledgebase entries and their versions. “No applicable evidence” requires an adequate, defined review scope; an incomplete dossier is insufficient evidence.

| Required field | Supplier |
|---|---|
| `variant` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `tumor_context` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `therapy_and_line` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `response_evidence` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `review_scope` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `evidence_applicability_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use a curated, versioned knowledgebase directly when the exact variant, tumor and therapy match and evidence scope is unambiguous.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One variant × therapy × patient tumor context × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use a curated, versioned knowledgebase directly when the exact variant, tumor and therapy match and evidence scope is unambiguous.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** This is evidence triage, not an autonomous treatment recommendation; no applicable evidence is not evidence of no treatment effect.

**Proposed validation:** Molecular-tumor-board or expert evidence labels; test tumor-context transfer, evidence conflicts and missed relevant studies.

<a id="pm01-synthetic-example"></a>

### PM01 synthetic example

SYNTHETIC: A study links variant V to sensitivity to therapy T in tumor A. The proposed patient context is tumor B; applicability criteria are absent.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [CIViC evidence types](https://docs.civicdb.org/en/latest/model/evidence/type.html)

<a id="pm02"></a>

## PM02

**Question:** Would feature [X] be available at the intended prediction time?

**Decision unit:** One clinical or laboratory feature × prediction time

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Available | The feature is accessible at or before prediction time throughout the defined record scope. |
| 2 | Unavailable | The feature is not accessible by prediction time throughout the defined record scope. |
| 3 | Availability varies across records | Known records or workflows include both available and unavailable instances; this is observed variation, not missing timestamps. |
| 4 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Feature definition, prediction time, measurement and result-release timestamps, clinical workflow and the defined record scope. Assess outcome leakage separately.

**Minimum context:** Feature definition, prediction time, measurement and result-release timestamps, clinical workflow and the defined record scope. Assess outcome leakage separately.

| Required field | Supplier |
|---|---|
| `feature_definition` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `prediction_time` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `measurement_and_availability_times` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `clinical_workflow` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `record_scope` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Compare trustworthy availability timestamps directly; use semantic review only for unresolved narrative workflow timing.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One clinical or laboratory feature × prediction time × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Compare trustworthy availability timestamps directly; use semantic review only for unresolved narrative workflow timing.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** Availability alone does not establish relevance or absence of other target leakage. Audit outcome-derived features and train/test leakage separately.

**Proposed validation:** Audit against actual availability logs; validate availability at intended deployment sites and times.

<a id="pm02-synthetic-example"></a>

### PM02 synthetic example

SYNTHETIC: A blood sample is collected at admission, but its assay result is released the next afternoon. The prediction is made at admission.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [scikit-learn data leakage and feature selection](https://scikit-learn.org/stable/common_pitfalls.html)

<a id="pm03"></a>

## PM03

**Question:** Does feature [X] represent a clinically relevant dimension not represented by the fixed selected feature set?

**Decision unit:** One candidate feature × fixed selected feature set × intended use

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Adds a distinct relevant dimension | The candidate represents a supported, relevant clinical dimension absent from the fixed selected set. |
| 2 | Represents an already-covered dimension | Its supported relevant dimension is already represented by the selected set. |
| 3 | Falls outside the intended prediction task | Its supported role does not address the intended prediction task. |
| 4 | Mixed or context-dependent relevance | Its role depends on context or includes both represented and additional relevant dimensions. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Selected-set snapshot, feature definitions, measurement timing, intended use and biological or clinical evidence.

**Minimum context:** Selected-set snapshot, feature definitions, measurement timing, intended use and biological or clinical evidence.

| Required field | Supplier |
|---|---|
| `intended_use` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `candidate_feature` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `selected_set_snapshot` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `feature_definitions` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `clinical_role_evidence` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use curated feature-role mappings when they resolve coverage; calculate redundancy and incremental predictive performance with statistical methods.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One candidate feature × fixed selected feature set × intended use × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use curated feature-role mappings when they resolve coverage; calculate redundancy and incremental predictive performance with statistical methods.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** Clinical complementarity is not statistical independence or extra predictive value. Do not discard useful correlated predictors on this answer alone.

**Proposed validation:** Blinded feature-role review and separate nested feature-selection evaluation with patient/site/time holdouts.

<a id="pm03-synthetic-example"></a>

### PM03 synthetic example

SYNTHETIC: The selected features measure renal function. Candidate X measures physical function; the prediction endpoint is not specified.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [scikit-learn data leakage and feature selection](https://scikit-learn.org/stable/common_pitfalls.html)

<a id="pm04"></a>

## PM04

**Question:** Does the supplied evidence support [medication or sample-handling factor] as an explanation for the abnormal [laboratory result]?

**Decision unit:** One laboratory abnormality × named medication or handling explanation

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supported as a sufficient explanation | Supplied analyses and contextual evidence support the named factor accounting for the full observed difference under prespecified sufficiency criteria. |
| 2 | Supported as a partial explanation only | Evidence supports a contribution, but also establishes that the named factor alone is insufficient under those criteria. |
| 3 | Evidence argues against this explanation | Adequate evidence is inconsistent with the named factor explaining the observed difference. |
| 4 | Conflicting evidence | Comparable evidence supports incompatible conclusions about the named factor. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Assay, specimen, measurement time, treatment or collection history and evidence for the specified interference or biological effect.

**Minimum context:** Assay, specimen, measurement time, treatment or collection history and evidence for the specified interference or biological effect.

| Required field | Supplier |
|---|---|
| `laboratory_result` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `assay_and_specimen` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `measurement_time` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `exposure_or_handling_history` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `mechanism_evidence` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `sufficiency_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use validated assay-interference rules when the exact method, exposure and timing fully determine applicability.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One laboratory abnormality × named medication or handling explanation × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use validated assay-interference rules when the exact method, exposure and timing fully determine applicability.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** A supported explanation does not rule out disease or authorize a treatment change. Evaluate each alternative factor separately.

**Proposed validation:** Expert laboratory dossiers with confirmed interferences and biological effects; assess missed competing explanations.

<a id="pm04-synthetic-example"></a>

### PM04 synthetic example

SYNTHETIC: A specimen was stored for 48 hours before analysis. The supplied interference note concerns a different assay; method equivalence is unknown.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.
