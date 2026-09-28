# Catalogue provenance and maintenance

The initial source is the user-supplied `Jev_Laya_Omics_Context_Catalogue (1).xlsx`, archived as `Jev_Laya_Omics_Context_Catalogue.xlsx` without changing its bytes. Its SHA-256 is `5e0f11c2363ebb296c8d397eff1d9c3c8e0ed234e8d06705a5a0ed2c1665bcbf`.

`Questions`, `Context_design` and `Routing` have headers on row 5 and one row per ID from rows 6–57. Join by ID, never by presumed row position. `questions.json` preserves all three original rows under `source_rows` and adds clearly identified editorial fields. `workbook-support.json` preserves Payload_schema, Validation and Sources; `workbook-start-here.json` preserves nonempty introductory rows with original positions. Dates are represented as ISO-style strings in textual exports. No formula or workbook value is edited.

The workbook is the archival authority for version 1 source wording. The integrated JSON is the maintained textual representation; domain Markdown pages are its readable derivatives. Do not maintain independent edits in three places.

For a correction: record a proposal and obtain scientific review of substantive changes; preserve the original workbook; increment question/catalogue versions as appropriate; update JSON and regenerate or carefully update its domain cards and examples; record changed fields in the changelog. Do not overwrite archival source cells to disguise a revision. If a later workbook becomes authoritative, retain a versioned copy and its checksum.

For every release, verify 52 unique IDs for this initial edition, exact source question/option cells, one matching context/routing row per ID, exact copied workbook hash, domain coverage, option aliases, JSON/YAML validity, local link targets/anchors and portable skill dependencies. Check all not-run outputs and examples for accidental claims of inference. Preserve verification notes separately from scientific validation. Temporary converters and validators may be used outside the repository and must not ship as execution infrastructure.

## Catalogue 1.1.0

The maintained catalogue now has 65 unique IDs across 13 domains. Verify the original 52 entries against the unchanged archive and verify the 13 additions against REV-009. Additions use `content_origin` and empty `source_rows`; never assign invented spreadsheet cells. Their operational definitions remain provisional. Derive domain cards and Supplementary Table S1 from the integrated JSON so wording, numbered options and definitions stay synchronized.

The current Supplementary Table S1 is published at `paper/supplementary_table_S1.docx` with a Markdown counterpart. These paper files are repository outputs, not skill dependencies; the portable skill uses only its own catalogue and references. The original 52-question local document and historical workbook are not the current expanded table.
