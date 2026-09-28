# Verification record

Date: 2026-09-28. Scope: documentation, source fidelity and preparation artifacts. **No Jev/Laya inference, training, calibration or biomedical validation was performed.**

## Catalogue 1.1.0 and Supplementary Table S1

- Verified 65 unique questions across 13 domains, including 13 new entries. All 52 original JSON entries are unchanged in full.
- Rechecked the archival workbook checksum and all 156 original question/context/routing row mappings. New entries have separate provenance and empty source rows.
- Confirmed exact agreement of question text, decision units, numbered labels and definitions across the JSON catalogue, domain cards and all 65 Word table rows. The Markdown table contains the same questions and options.
- Parsed catalogue JSON and checked context required fields, types, option IDs and missingness lists. The existing schema files were not changed; a full external JSON Schema validator was unavailable for this update, so context-envelope checks were performed directly.
- Checked relative skill links and anchors, including a standalone copied skill folder. The new project guide maps 25 editorial examples to 13 new and 12 existing questions.
- Confirmed that the new example passages are synthetic and the new contexts are explicitly unmapped/incomplete. No answer or probability was added; existing demonstration results remain not run.
- Rendered the updated Word table and visually reviewed all 17 pages. Increased table text to 9 pt, reconciled column widths, repeated the header and kept each question row together. Only document content and field-update settings changed in the source Word package; other package parts were preserved.
- Kept temporary authoring, validation and rendering files outside the repository. Unrelated local manuscript drafts were not included in this publication.

All operational definitions remain provisional. These checks establish consistency and document readability, not scientific validity or model performance.

## Initial 1.0.0 checks

- The archived workbook is byte-identical to the user-supplied file. SHA-256: `5e0f11c2363ebb296c8d397eff1d9c3c8e0ed234e8d06705a5a0ed2c1665bcbf`.
- All 52 unique question IDs have exactly matching original rows in Questions, Context_design and Routing: 156 full row comparisons. Question wording and numbered option labels match exactly.
- The 11 domain pages preserve question text, options and the structured examples in the integrated JSON.
- JSON and YAML files parse. All four JSON Schemas pass Draft 2020-12 schema checks; templates, catalogue contexts and worked examples are checked against the applicable envelopes.
- The three worked demonstrations contain 15 contexts and 15 corresponding unexecuted result records. Required field names, missing-field lists, selected options and evidence links are checked. All answers and probabilities remain null.
- Relative Markdown links and anchors are checked. A temporary copy of the skill is checked independently, without repository-root dependencies or symlinks.
- The bundled skill validator accepts the skill manifest. The file inventory contains only documentation, declarative files, CSV and the original XLSX, plus repository housekeeping.

Temporary conversion and validation tools and their dependencies were kept outside the repository. The workbook was read and copied, not edited or re-rendered.

## Manual preparation review

| User starting point | Preparation supported | Limit retained |
|---|---|---|
| Panel membership and assay passages | BM01–BM03; source-to-field mapping; separate role, matrix and cohort evidence | No invented incremental AUC or analytical acceptance threshold |
| Annotated variant and functional-assay passage | GV01/GV03/GV04; transcript and mechanism mapping; distinction between prediction and experiment | No pathogenicity verdict; missing transcript remains a gap |
| Short epidemiological notes | EC01/EC04/EC05; episode, experiencer and ordered-status fields; rule-first bypass | Missing history is not new onset; unresolved same-time conflict requires review |

This is an author review of the static preparation workflow, not an independent assistant benchmark or a model execution. Original brief vignettes are often incomplete; editorial mappings retain nulls and qualifiers. Expanded demonstrations are clearly synthetic additions.

## Remaining scientific and implementation work

Operational option definitions and overlap/precedence proposals need domain-expert review. Token counts are unverified because no tokenizer/checkpoint was loaded. The Laya README/packing discrepancy needs runtime-specific checking. Actual adapters, training recipes, data authorization, evaluation labels, calibration and biomedical validation remain future work.
