# Proposed revisions and unresolved definitions

No original question or option has been silently changed. The workbook contains numbered labels rather than full operational rubrics. This edition adds proposed definitions, field suppliers and missingness handling; all require expert review before labeling real evaluation data.

| Proposal | Affected IDs | Issue | Proposed review action |
|---|---|---|---|
| REV-001 | All | Numbered options coexist with an illustrative A/B/C/D/U payload and U routing shorthand. | Keep numeric strings as canonical IDs scoped by question; resolve U by exact `Insufficient evidence` label. Any backend alias must be reversible. EC03 and EC05 use option 6 for abstention. |
| REV-002 | EC01, TH05, GV04, SC02, PR01, HI04 | An unresolved substantive category can overlap with insufficient evidence. | Define sufficient documentation to conclude that the source itself leaves the issue unresolved; missing decisive source material belongs to abstention. Adjudicate boundaries on real cases. |
| REV-003 | GV05 | The source example combines young age with absent formal audiometry; options 2 and 3 can both apply. | Draft an expert-approved precedence rule. Do not assign a reference label from the vignette alone. |
| REV-004 | TH04 | Broad cell loss, unresolved toxicity and absent specificity tests can co-occur. | Specify whether observed broad loss takes precedence and which controls establish a target contribution. |
| REV-005 | ML02 | “Exposure not documented” differs from insufficient evidence only if history coverage is known. | Require an explicit reviewed-history scope; inaccessible or partial history must not imply a documented absence. |
| REV-006 | TH02 | No relationship in one retrieved item is not literature-wide absence. | Keep the item-level scope and retrieval coverage visible; never relabel it as novelty. |
| REV-007 | All synthetic vignettes | Short workbook examples do not populate every required field. | Preserve original text, map only explicit facts and show all remaining gaps. Use expanded synthetic demonstrations only when clearly identified. |
| REV-008 | Laya integration | README describes an option-budget error while the pinned packer truncates text. | Inspect and test the chosen caller/configuration during future implementation before claiming length validation. |

## REV-009 — Multiscale project coverage in catalogue 1.1.0

On 2026-09-28, the repository owner requested incorporation of the Figure 2 editorial proposals into Supplementary Table S1 and the GitHub resource. Editorial adoption adds 13 proposed tasks: EC06–EC11, PM01–PM04, HI05–HI06 and RI01. This is authorization to publish the proposed content, not recorded domain validation or an independent scientific review.

The original 52 entries are unchanged. The [project guide](project-use-cases.md) maps the other 12 editorial examples to existing questions. PM02 is limited to feature availability; outcome-derived information and other leakage mechanisms require separate checks. EC11 records both event and information-availability timing. New options include explicit operational boundaries; the case hierarchy, sufficiency criteria, tissue-assay validity precedence and definition of an adequate evidence review need domain review before annotation.

New entries have author-request provenance, question version 1.0.0, empty `source_rows` and no claimed original workbook cells. The catalogue version is 1.1.0; the archival workbook remains the authority for its original 52 entries. New contexts are synthetic, deliberately incomplete and unexecuted. There are no new reference labels or model outputs.

Scientific adoption of provisional definitions requires a named domain reviewer, rationale, affected IDs and corresponding version/derivative updates. This release does not silently promote editorial definitions to validated criteria.
