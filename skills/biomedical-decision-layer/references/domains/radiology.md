# Radiology

Proposed research tasks, introduced in catalogue 1.1.0. No model execution or biomedical validation has been performed.

[RI01](#ri01)

<a id="ri01"></a>

## RI01

**Question:** Do the reported CT or chest X-ray findings meet the study’s imaging criteria for [target lesion or pattern]?

**Decision unit:** One CT or X-ray study × target imaging-pattern criteria

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Meets criteria | Usable reported findings satisfy the modality-specific target criteria. |
| 2 | Does not meet criteria | Adequate reported findings fail the specified criteria. |
| 3 | Conflicting findings | Comparable usable interpretations for the same target and examination conflict. |
| 4 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Modality-specific criteria, radiology report or extracted findings, anatomy, acquisition quality and relevant prior images. Define separate criteria for CT and radiography where needed.

**Minimum context:** Modality-specific criteria, radiology report or extracted findings, anatomy, acquisition quality and relevant prior images. Define separate criteria for CT and radiography where needed.

| Required field | Supplier |
|---|---|
| `modality_and_anatomy` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `reported_findings` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `target_pattern_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `acquisition_quality` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `comparison_images` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use validated structured reporting or imaging classifiers when they already resolve the target criteria.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One CT or X-ray study × target imaging-pattern criteria × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use validated structured reporting or imaging classifiers when they already resolve the target criteria.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** No direct pixel interpretation, segmentation or lesion measurement is assumed. Imaging compatibility does not by itself establish pathogen identity.

**Proposed validation:** Radiologist-adjudicated reports/findings; split by patient and site, stratify by modality and evaluate image-to-descriptor errors separately.

<a id="ri01-synthetic-example"></a>

### RI01 synthetic example

SYNTHETIC: A CT report describes a focal opacity. The study’s target-lesion criteria and image-quality assessment are not supplied.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [WHO chest imaging recommendations](https://www.ncbi.nlm.nih.gov/books/NBK586653/)
