# Preparation templates

Fill templates from supplied data; nulls mark unfinished information. These repository records are not executable API payloads.

- [Study brief](study-brief.yaml): question, scope and available material.
- [Data dictionary](data-dictionary.csv): one row per source-to-field mapping.
- [Evidence record](evidence-record.json): complete source provenance and extraction history.
- [Decision context](decision-context.json): selected fields for one atomic decision.
- [Decision result](decision-result.json): unexecuted output envelope; all result values remain null.
- [Training example](training-example.json): separate proposed and reviewed labels, dependence groups and splits.
- [Implementation handoff](implementation-handoff.md): later implementation requirements and open questions.

Companion [context](decision-context.schema.json), [result](decision-result.schema.json), [evidence](evidence-record.schema.json) and [training](training-example.schema.json) JSON Schemas validate structure. They permit nulls in drafts; catalogue completeness, semantic sufficiency, source access and option membership need separate review. Do not call a blank template decision-ready.
