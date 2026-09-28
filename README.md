# Jev biomedicine

Prepare bounded biomedical research questions from your data, with traceable evidence and explicit answer options. This repository provides **52 proposed questions across 11 domains**, a portable assistant skill, templates, and synthetic examples.

**Status:** documentation and preparation only. There are no model runs, validated biomedical capabilities, diagnostic claims, or measured savings. The skill helps prepare an analysis; it is not Jev or Laya and does not perform inference.

```text
Research data → scripts / specialist tools → structured context
             → Jev / Laya → external routing rules → LLM / expert
```

An optional upstream LLM can extract reusable facts once per source and biological context. Each decision remains atomic; application rules combine answers later.

## Start with your question and files

No JSON preparation is required. Give an assistant your research question and a small, authorized data sample. It should return selected catalogue IDs, a column-to-context map, example inputs, missing evidence, and an implementation handoff.

Copy this prompt into an assistant with authorized access to this repository:

> Read https://github.com/hylixinsights/Jev-biomedicine, starting with its README and skills/biomedical-decision-layer/SKILL.md. My research question is [QUESTION], and my available data are [FILES OR DESCRIPTIONS]. Select relevant catalogue questions, map my data to their required context, identify missing evidence, and prepare example decision inputs. Do not run models, install packages, or send data to external services.

Repository access retrieves documentation; it does not install the skill. See [access and local skill setup](skills/biomedical-decision-layer/references/getting-started.md) for verified platform guidance. If repository retrieval is unavailable, provide the skill folder and relevant domain files locally.

## Explore

- [Question catalogue: 11 domains](skills/biomedical-decision-layer/references/catalogue-index.md)
- [Assistant skill](skills/biomedical-decision-layer/SKILL.md)
- [Context and provenance contract](skills/biomedical-decision-layer/references/context-design.md)
- [Short contexts and aggregation](skills/biomedical-decision-layer/references/chunking-and-aggregation.md)
- [Worked examples](skills/biomedical-decision-layer/assets/examples/index.md): biomarkers, variants, epidemiology, and examples for every domain
- [Preparation templates](skills/biomedical-decision-layer/assets/templates/index.md)
- [Validation plan](skills/biomedical-decision-layer/references/validation.md)

## When you are ready to run the models

Read [future integration](skills/biomedical-decision-layer/references/future-integration.md) and [fine-tuning preparation](skills/biomedical-decision-layer/references/fine-tuning.md). These are optional implementation guides. This version contains no clients, scripts, notebooks, servers, model weights, or execution infrastructure. Its declarative contracts are not official API schemas.

## Provenance and maintenance

The [original Excel workbook](skills/biomedical-decision-layer/assets/catalogue/Jev_Laya_Omics_Context_Catalogue.xlsx) is preserved byte for byte. Its three question-related sheets are joined by ID in [questions.json](skills/biomedical-decision-layer/assets/catalogue/questions.json). All question wording, option numbering and labels remain unchanged. New operational definitions are editorial proposals awaiting expert review.

See [source register](skills/biomedical-decision-layer/references/sources.md), [revision proposals](skills/biomedical-decision-layer/references/catalogue-revisions.md), [maintenance procedure](skills/biomedical-decision-layer/references/catalogue-maintenance.md), and [verification report](VERIFICATION.md). No project license or DOI has been assigned. Upstream projects retain their own terms.

Public examples are synthetic. Keep identifiable research data out of GitHub. Nothing in this package authorizes automatic external data transfer or a final clinical decision.
