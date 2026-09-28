# Getting started

## Read through GitHub

An assistant needs authorized access to the files, not an installed model. In the current OpenAI interface, open the Plugins directory, select GitHub, install it if needed, and connect the account when prompted. Then start a new chat and explicitly ask it to read this repository. Access depends on account and repository permissions. See the official [Plugins guide](https://learn.chatgpt.com/docs/plugins), checked 2026-09-28.

The GitHub connection supplies tools for accessing authorized content. It does not automatically install this repository's instructions as a skill. A public URL alone does not guarantee that every assistant can retrieve every file. If retrieval fails, provide local copies of SKILL.md and its supporting folder.

## Install a local Codex skill

Review the folder first. Copy the **entire** `biomedical-decision-layer` folder from this repository's `skills` directory to a supported local skills directory. Current official Codex documentation lists `.agents/skills` inside a project and `$HOME/.agents/skills` for personal use. Invoke it as `$biomedical-decision-layer` in Codex CLI or the IDE skill selector. Restart if it is not discovered. Alternatively, ask the built-in skill installer to install the folder from this repository. These are local setup choices, not actions performed by this package.

Source: [Build skills](https://learn.chatgpt.com/docs/build-skills), checked 2026-09-28. No universal installation command, plugin manifest or automatic cross-platform compatibility is claimed. The generic package follows the [Agent Skills specification](https://agentskills.io/specification); other hosts require their own verified setup.

## Begin with ordinary data

For example: “I am selecting plasma biomarker evidence for a severe-versus-mild comparison. I have a gene table, panel membership and two assay passages.” The assistant can select BM01–BM03, inspect the supplied columns, and show gaps before asking for more material. It must not assume differential expression establishes protein detection or matrix validity.

Provide the intended use, a small authorized sample and descriptions of existing analyses. The assistant fills the preparation templates. Keep actual private records in approved storage; use synthetic examples for public discussion.

Completion means the researcher can inspect a source-to-field map, proposed contexts and a handoff. It does not require inference, package installation or JSON expertise.
