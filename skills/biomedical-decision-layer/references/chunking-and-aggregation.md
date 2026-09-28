# Short contexts and aggregation

Retrieval segmentation locates evidence in long documents. Final input assembly gathers the facts needed for one question. They are different operations. Do not classify arbitrary 512-token chunks and treat their majority as a document-level answer.

## Backend limits are version-specific

The [receptron README at the inspected commit](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/README.md) documents an English `max_len` of 512 and `head_max_len` of 192. The state shares the total packed sequence with the question header and options; 512 is not a free document budget. Other checkpoints differ. Consult the pinned checkpoint configuration and tokenizer rather than assuming a universal limit.

The [packing source](https://github.com/receptron/laya/blob/6478649e723122ca24bbf5fb69ed1010023c9750/src/sequence.ts) constructs a sequence with special tokens, question text, marked options and serialized state. It initially clips option text to 48 tokens, may shorten options further to allocate header space, truncates the header, and fits state into remaining room. The README describes an option-budget error; the inspected packing function performs truncation. We did not execute the runtime or establish every caller's validation. Resolve this discrepancy for the exact runtime before relying on either behavior.

No tokenizer or weights were downloaded for these examples. Every token count here is **unverified**. Future preparation must measure the actual packed sequence with the selected tokenizer revision, serialization rules, configuration, option ordering and special tokens. Record state length, header/options length, packed length, retained text and truncation. Reject an input that loses a decisive fact or changes an option's meaning.

## Assemble evidence

Keep entity identity, negation, comparators, units, experimental conditions and temporal anchors together. Select methods/results spans rather than a discussion's conclusion. Preserve the distinction between an unperformed measurement and a negative measurement. Store omitted material and retrieval coverage in the full evidence record. Shorten repetition, not decisive qualifiers.

Synthetic transformation for BM02:

1. **Long source:** a product document contains catalog history, buffer experiments, serum measurements and a broad “blood samples” heading.
2. **Relevant evidence:** E1 = Methods paragraph 2, recombinant protein spiked into buffer; E2 = Results paragraph 1, endogenous serum measurements. Both belong to experiment group X1 and must not count as independent replication.
3. **Compact state:** “Target EDTA plasma; preparation follows planned plasma SOP. X1/E1 tests recombinant protein in buffer. X1/E2 measures endogenous protein in serum. Supplied complete validation sections report no EDTA-plasma experiment.”
4. **Question and options:** BM02, with original numbered labels from the [biomarker cards](domains/transcriptomics-biomarkers.md#bm02). No illustrative label is inserted into this state.

The complete [biomarker demonstration](../assets/examples/biomarkers/README.md) shows field mapping. This compression illustrates content selection, not a verified token budget.

## Specify aggregation per question

| Question | Evidence grouping and combination policy |
|---|---|
| BM02 | Group excerpts by assay, matrix, preparation and experiment. A buffer experiment cannot vote down direct plasma evidence. Review contradictions between comparable plasma experiments. Keep matrix applicability separate from BM01 complementarity and statistical performance. |
| GV01 / GV03 | Keep variant, transcript and gene–disease mechanism fixed. Evaluate mechanism compatibility separately from whether an assay tests it. Do not average predictions with experimental findings. Review contradictions between relevant functional experiments. |
| EC01 / EC05 | Preserve the participant, episode and ordered timeline. A discharge statement may resolve an earlier suspicion for a discharge-time question; unresolved contemporaneous contradictions require review. Do not majority-vote repeated copied notes. |
| HI03 / BM01 | Compare each candidate with one frozen selected-set snapshot. Recompute preparation when the selected set changes; candidates evaluated against different snapshots are not interchangeable. |

For other questions, use the card's decision unit and routing proposal to write an explicit policy before execution. Overlapping excerpts, duplicated reports and repeated notes share an experiment/document dependence ID. Do not treat them as independent support, multiply their probabilities, or automatically average them.

If decisive comparisons cannot fit together, first extract reusable factual records with source spans. Decompose only into genuinely independent questions, then combine by an explicit rule. If compression removes the scientific comparison, send the complete evidence to deeper analysis. An empty retrieval chunk means “not found here,” not “absent from the document” or “negative biology.”
