# Future integration, without execution

This package contains no adapter or model runner. Resolve the following interface details when implementation is explicitly requested. Sources were checked on 2026-09-28; see the [register](sources.md).

## Jev through TypeSafe

The official [API reference](https://docs.typesafe.ai/api) documents `POST https://api.typesafe.ai/v1/systemone` with bearer authentication. Input contains a selected `model`, a text/object/array `state`, and a map of typed `questions`. Responses contain the model, keyed answers and usage. The catalogue's finite categories map naturally to `choice`: `instructions` holds the question, and `criteria` maps stable option keys to definitions. Preserve a reversible mapping from repository numeric-string option IDs. See [Choice](https://docs.typesafe.ai/primitives/choice).

The adapter must keep audit metadata outside the minimal state, validate every returned option, and preserve raw responses. Pin a supported model version for evaluation and record the actual returned model rather than treating an alias as a reproducible version. Validate current cardinality, body and request limits before deployment. No API request has been made.

Credentials belong in approved secret storage or runtime environment configuration, never in public templates, browser assets or logs. Check data authorization, retention terms and destination before sending any research content. Missing credentials are an implementation prerequisite, not something this skill requests during preparation.

Provider confidence is distinct from maximum class probability and task-specific reliability. TypeSafe's [confidence documentation](https://docs.typesafe.ai/confidence) explains its distribution-derived quantity; biomedical routing still requires held-out calibration and error analysis. Hosted Jev fine-tuning support was not established by the consulted documentation. Do not promise it.

## Laya locally

Entry point: [receptron/laya](https://github.com/receptron/laya), inspected commit `6478649e723122ca24bbf5fb69ed1010023c9750`, package `@receptron/laya` version `0.1.2`. Its Node/TypeScript inference package uses ONNX Runtime. The [pinned README](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/README.md) documents Node 20+, first-use download of roughly 1.7 GB of ONNX weights, a cache under `~/.cache/receptron-laya`, and approximately 2 GB model RAM plus batch overhead. These are upstream estimates, not measurements here.

Distinguish the inference package from the [Convai Innovations checkpoint](https://huggingface.co/convaiinnovations/laya) and the [upstream Python/training repository](https://github.com/NandhaKishorM/laya). The Node package is not a training toolkit. Its exported ONNX bundle and tokenizer/configuration must correspond to the chosen checkpoint revision; package version alone does not pin weights. A first local load can involve a network download. Plan offline provisioning separately if required.

Inspect the [packing limitations](chunking-and-aggregation.md). A claimed Jev-compatible interface does not establish equivalent biomedical answers, calibration or error behavior. Confirm request serialization, supported question types, returned probability keys and truncation independently for each backend.

## Implementation acceptance specification

Prepare a mapping table from repository context to backend state, question text to instructions, and numeric option IDs to criteria keys. Verify that all original categories, including insufficient evidence, survive packing. Validate response types, known IDs, finite probabilities within bounds and normalization under a documented tolerance. Preserve raw and calibrated values separately.

Missing required facts, unresolved identity, technical QC failure and out-of-scope inputs must stop automatic routing regardless of confidence. Keep `not_run` until an actual response exists; failures carry an error record, not fabricated probabilities. Specify request limits, timeout/retry policy and storage before implementation. Measure retrieval, preparation, inference and review separately. Use [validation](validation.md) to decide whether the layer improves the research workflow.
