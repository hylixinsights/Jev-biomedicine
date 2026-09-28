# Epidemiology

Proposed research tasks, version 1.0.0. Every example is synthetic. No question has been run or scientifically validated. Option definitions below are editorial proposals for expert review; numbered IDs and labels are preserved.

[EC01](#ec01) | [EC02](#ec02) | [EC03](#ec03) | [EC04](#ec04) | [EC05](#ec05)

Use the [context contract](../context-design.md) for missingness and provenance, and the [revision register](../catalogue-revisions.md) for unresolved category overlaps.

<a id="ec01"></a>

## EC01

**Question:** Does the passage place this condition before the current episode or describe new onset during it?

**Decision unit:** One condition mention × current illness episode

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Pre-existing condition | The passage explicitly places the condition before the index episode. |
| 2 | New onset during current episode | The passage explicitly places first onset during the index episode. |
| 3 | Pre-existing condition with current worsening | A pre-existing condition is explicitly described as worsening during the episode. |
| 4 | Timing remains unresolved | The available narrative leaves temporal ordering unresolved. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** De-identified narrative + episode anchors + rule-based annotations

**Minimum context:** Original passage with nearby temporal context; named condition; episode start; explicit negation and experiencer information.

| Required field | Proposed supplier |
|---|---|
| `condition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `original_passage` | Source retrieval; optional cached factual extraction; human source check |
| `episode_start` | Provided study metadata or source record; researcher confirms identity and meaning |
| `surrounding_sentences` | Provided study metadata or source record; researcher confirms identity and meaning |
| `rule_results` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** De-identify, segment and apply dictionaries/ConText-like rules; pass only unresolved temporal relations with the original text.

**Optional cached LLM extraction:** Not needed for short passages. For long documents, extract relevant spans without assigning the final temporal category.

**Reuse key:** rule set and definition reusable; context assembled per record

**Semantic work remaining:** Resolves event relations and changes across sentences after simple trigger rules fail.

**Bypass Jev/Laya:** Explicit dates and reliable lexical rules should answer directly; no model call for obvious 'history of' cases.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Apply the approved coding dictionary to each independent answer; preserve unknown, denied, historical and hypothetical statuses.

**Future routing:** Pre-existing/new/worsening → research label after validated policy; unresolved/U → retrieve context or human review.

**Expert follow-up:** Review original record under the study's annotation manual; no autonomous clinical action.

**Interpretation limit:** New onset during an episode is not proof that the disease caused the condition.

**Proposed validation:** Double-annotated de-identified passages; split by patient, site and time; measure minority-class recall.

### EC01 synthetic example

Input material: Surveillance/clinical free text, encounter dates and rule-based NLP output.

SYNTHETIC: 'Renal disease had been under follow-up for years. During this admission, renal function deteriorated and dialysis was started.' Query condition: renal disease; index episode: current admission.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EC01",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ec01",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EC01-E1",
      "source_id": "workbook-synthetic-EC01",
      "source_location": "Context_design!G45",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: 'Renal disease had been under follow-up for years. During this admission, renal function deteriorated and dialysis was started.' Query condition: renal disease; index episode: current admission."
    }
  ],
  "context_fields": {
    "condition": "renal disease",
    "original_passage": "Renal disease had been under follow-up for years. During this admission, renal function deteriorated and dialysis was started.",
    "episode_start": "current admission",
    "surrounding_sentences": null,
    "rule_results": null
  },
  "missing_fields": [
    "surrounding_sentences",
    "rule_results"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing condition from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 45, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

<a id="ec02"></a>

## EC02

**Question:** Does the exposure described in the passage meet the study's operational exposure definition?

**Decision unit:** One exposure passage × study definition

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Definition met | Documented exposure meets all operational criteria in the specified time window. |
| 2 | Related but nonqualifying exposure | A related exposure is described but fails at least one required criterion. |
| 3 | Qualifying exposure explicitly denied | The qualifying exposure is explicitly denied. |
| 4 | Hypothetical exposure only | The passage describes a possible or hypothetical exposure, not an actual event. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Original narrative + explicit operational definition

**Minimum context:** Definition with inclusion/exclusion conditions; passage and limited surrounding context; any already-computed time-window result.

| Required field | Proposed supplier |
|---|---|
| `exposure_definition` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `original_passage` | Source retrieval; optional cached factual extraction; human source check |
| `time_window_result` | Existing statistical or specialist output; record tool, version, units and QC |
| `rule_results` | Existing statistical or specialist output; record tool, version, units and QC |
| `context_sentences` | Provided study metadata or source record; researcher confirms identity and meaning |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Apply exact dictionary, negation and date-window rules first; attach the unresolved passage and the relevant definition clause.

**Optional cached LLM extraction:** Not needed per record. A one-time protocol extraction may draft the definition, but a domain expert must approve it.

**Reuse key:** definition version reusable across records; candidate text per record

**Semantic work remaining:** Matches an operational definition to a described situation rather than looking for an exposure keyword.

**Bypass Jev/Laya:** If location/contact/time facts are structured and sufficient, evaluate the definition in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Apply the approved coding dictionary to each independent answer; preserve unknown, denied, historical and hypothetical statuses.

**Future routing:** Met/nonqualifying/denied/hypothetical → retain separate research labels; U → no negative assignment; review.

**Expert follow-up:** Review original record under the study's annotation manual; no autonomous clinical action.

**Interpretation limit:** The answer reflects the supplied definition and record, not the person's actual exposure beyond the record.

**Proposed validation:** Expert labels under a frozen protocol; include near-miss scenarios and incomplete records.

### EC02 synthetic example

Input material: Exposure narratives, study protocol, encounter dates and rule-based entity/negation output.

SYNTHETIC: The study requires sharing an enclosed room with a confirmed case. The passage says the person worked at the same company but on a separate remote shift and never met the case.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EC02",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ec02",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EC02-E1",
      "source_id": "workbook-synthetic-EC02",
      "source_location": "Context_design!G46",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: The study requires sharing an enclosed room with a confirmed case. The passage says the person worked at the same company but on a separate remote shift and never met the case."
    }
  ],
  "context_fields": {
    "exposure_definition": "sharing an enclosed room with a confirmed case",
    "original_passage": "Worked at the same company on a separate remote shift and never met the case.",
    "time_window_result": null,
    "rule_results": null,
    "context_sentences": null
  },
  "missing_fields": [
    "time_window_result",
    "rule_results",
    "context_sentences"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing exposure_definition from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 46, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

<a id="ec03"></a>

## EC03

**Question:** What reason for non-vaccination is explicitly supported by the passage?

**Decision unit:** One documented reason for non-vaccination

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Access barrier only | Access obstacles are the sole documented reason. |
| 2 | Voluntary non-vaccination only | A voluntary decision is the sole documented reason. |
| 3 | Medical deferral only | Medical deferral is the sole documented reason. |
| 4 | Multiple reasons | Two or more reasons are explicitly documented. |
| 5 | Other specified reason | A documented reason falls outside the first three categories. |
| 6 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** De-identified narrative + reason-category definitions

**Minimum context:** Original passage; relevant subject/time; category definitions distinguishing logistics, choice and documented medical deferral.

| Required field | Proposed supplier |
|---|---|
| `original_passage` | Source retrieval; optional cached factual extraction; human source check |
| `subject` | Provided study metadata or source record; researcher confirms identity and meaning |
| `time_context` | Provided study metadata or source record; researcher confirms identity and meaning |
| `reason_definitions` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `rule_results` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Segment, remove identifiers and run lexical/rule baselines; provide the original wording of unresolved reasons.

**Optional cached LLM extraction:** No LLM required for individual passages; use human-approved category definitions.

**Reuse key:** category definitions reusable; no per-record LLM extraction

**Semantic work remaining:** Distinguishes expressed intent and practical barriers from keyword-based assumptions about refusal.

**Bypass Jev/Laya:** Explicit structured reasons or reliable rules should bypass the model.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Apply the approved coding dictionary to each independent answer; preserve unknown, denied, historical and hypothetical statuses.

**Future routing:** Assign only the supported research category; multiple → preserve all documented reasons; U → unknown, not refusal.

**Expert follow-up:** Review original record under the study's annotation manual; no autonomous clinical action.

**Interpretation limit:** Do not infer beliefs, motives, medical appropriateness or a reason from non-vaccination alone.

**Proposed validation:** Experts label explicit reasons; evaluate access-versus-choice errors and dialect/language differences.

### EC03 synthetic example

Input material: Surveillance/questionnaire free text and a research annotation manual.

SYNTHETIC: 'She intended to receive the vaccine, but the only appointment was during work and transport was unavailable.' No refusal or medical deferral is described.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EC03",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ec03",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EC03-E1",
      "source_id": "workbook-synthetic-EC03",
      "source_location": "Context_design!G47",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: 'She intended to receive the vaccine, but the only appointment was during work and transport was unavailable.' No refusal or medical deferral is described."
    }
  ],
  "context_fields": {
    "original_passage": "She intended to receive the vaccine, but the only appointment was during work and transport was unavailable.",
    "subject": "woman described by the synthetic passage",
    "time_context": null,
    "reason_definitions": null,
    "rule_results": null
  },
  "missing_fields": [
    "time_context",
    "reason_definitions",
    "rule_results"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing original_passage from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 47, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

<a id="ec04"></a>

## EC04

**Question:** Whose condition or exposure is described by this passage?

**Decision unit:** One condition/exposure mention × person in the record

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Index participant | The mention refers to the index participant. |
| 2 | Another person | The mention refers to another identifiable role in the record. |
| 3 | Both index participant and another person | The condition or exposure is explicitly attributed to both. |
| 4 | Generic or hypothetical person | The mention concerns a generic or hypothetical person. |
| 5 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Original narrative + subject identifiers replaced with roles

**Minimum context:** Mention of interest; preceding/following sentences; consistent role placeholders and rule-based experiencer candidates.

| Required field | Proposed supplier |
|---|---|
| `target_mention` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `passage` | Source retrieval; optional cached factual extraction; human source check |
| `role_map` | Provided study metadata or source record; researcher confirms identity and meaning |
| `surrounding_sentences` | Provided study metadata or source record; researcher confirms identity and meaning |
| `rule_experiencer` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Apply explicit family-history/subject rules; preserve role relationships during de-identification; route only ambiguous references.

**Optional cached LLM extraction:** Not needed for bounded passages. Do not replace text with a summary that has already resolved the referent.

**Reuse key:** rule library reusable; passage-specific context

**Semantic work remaining:** Resolves cross-sentence reference or embedded clauses that simple nearby-keyword rules can misattribute.

**Bypass Jev/Laya:** Use ConText-like experiencer rules when they already resolve the subject.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Apply the approved coding dictionary to each independent answer; preserve unknown, denied, historical and hypothetical statuses.

**Future routing:** Participant/other/both/generic → separate research labels; U → do not add a participant condition; review.

**Expert follow-up:** Review original record under the study's annotation manual; no autonomous clinical action.

**Interpretation limit:** De-identification must preserve subject relationships; do not infer an individual's condition from a relative's condition.

**Proposed validation:** Expert experiencer labels; include long clauses and multiple family members.

### EC04 synthetic example

Input material: De-identified free text, sentence boundaries and coreference/rule output.

SYNTHETIC: 'The participant lives with her mother, who has diabetes. She reports that her own screening was normal.' Query condition: diabetes.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EC04",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ec04",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EC04-E1",
      "source_id": "workbook-synthetic-EC04",
      "source_location": "Context_design!G48",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: 'The participant lives with her mother, who has diabetes. She reports that her own screening was normal.' Query condition: diabetes."
    }
  ],
  "context_fields": {
    "target_mention": "diabetes",
    "passage": "The participant lives with her mother, who has diabetes. She reports that her own screening was normal.",
    "role_map": {
      "participant": "index person",
      "mother": "another person"
    },
    "surrounding_sentences": null,
    "rule_experiencer": null
  },
  "missing_fields": [
    "surrounding_sentences",
    "rule_experiencer"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing target_mention from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 48, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

<a id="ec05"></a>

## EC05

**Question:** What status does the passage explicitly assign to the condition at the relevant time?

**Decision unit:** One condition × evolving diagnostic statement

| Original option ID | Original label | Proposed operational definition |
|---|---|---|
| 1 | Documented as confirmed | The record explicitly documents confirmation at the specified time. |
| 2 | Suspected or under investigation | The condition remains suspected or under investigation at that time. |
| 3 | Explicitly ruled out | The condition is explicitly ruled out at that time. |
| 4 | Historical diagnosis only | Only a historical diagnosis is documented, without current confirmation. |
| 5 | Conflicting statements | Statements at the relevant time conflict without a resolved sequence. |
| 6 | Insufficient evidence | Decisive facts, identity, source coverage or usable context are missing. Do not interpret this as a negative finding. |

**Accepted inputs:** Narrative + reference time + negation/status rule output

**Minimum context:** Condition; relevant encounter/time; source statements and any explicit later revision; operational status definitions.

| Required field | Proposed supplier |
|---|---|
| `condition` | Provided study metadata or source record; researcher confirms identity and meaning |
| `relevant_time` | Provided study metadata or source record; researcher confirms identity and meaning |
| `ordered_passages` | Source retrieval; optional cached factual extraction; human source check |
| `status_definitions` | Researcher or expert-approved study protocol; scripts preserve the fixed definition |
| `rule_results` | Existing statistical or specialist output; record tool, version, units and QC |

Required means represented by a traceable value or explicit missingness, not invented completeness. Use null for unknowns. Optional audit fields: `retrieval_coverage`, `source_version`, `computed_result_provenance`, `selected_set_version`; include them when relevant.

**Upstream preparation:** Order documents by time; resolve explicit negation/status terms first; retain conflicting passages without collapsing them.

**Optional cached LLM extraction:** For long records, retrieve source spans only. Do not ask an upstream LLM to decide the final status and then repeat the task.

**Reuse key:** status definitions reusable; episode-specific ordered passages

**Semantic work remaining:** Interprets revisions in the narrative while preserving the distinction between reported and independently verified diagnoses.

**Bypass Jev/Laya:** If structured status/date fields are authoritative and complete, use them in code.

**Abstain or escalate:** Missing/truncated decisive context; incompatible IDs; input outside validated domain; weak or conflicting answers. No universal confidence threshold: calibrate on held-out task data.

**Independent evaluation:** EC: shared candidate dossier; evidence-item questions batched by item

**Combine outside the model:** Apply the approved coding dictionary to each independent answer; preserve unknown, denied, historical and hypothetical statuses.

**Future routing:** Return a documentation-status research label; conflicting/U → record unresolved status and review.

**Expert follow-up:** Review original record under the study's annotation manual; no autonomous clinical action.

**Interpretation limit:** 'Documented as confirmed' describes the record, not an independent diagnosis by the model.

**Proposed validation:** Expert time-specific assertion labels; hold out institutions/document templates.

### EC05 synthetic example

Input material: De-identified reports, encounter chronology and rule-based assertion annotations.

SYNTHETIC: 'Initially treated as possible pneumonia. Subsequent assessment ruled out pneumonia; symptoms were attributed to another condition.' Query time: discharge.

Assembled provenance wrapper (a field-mapping exercise, not a complete backend request):

```json
{
  "synthetic": true,
  "question_id": "EC05",
  "question_version": "1.0.0",
  "candidate_id": "synthetic-ec05",
  "context_version": "1.0.0",
  "study_context": {},
  "computed_results": [],
  "evidence_items": [
    {
      "evidence_id": "EC05-E1",
      "source_id": "workbook-synthetic-EC05",
      "source_location": "Context_design!G49",
      "extraction_method": "verbatim workbook transcription",
      "excerpt": "SYNTHETIC: 'Initially treated as possible pneumonia. Subsequent assessment ruled out pneumonia; symptoms were attributed to another condition.' Query time: discharge."
    }
  ],
  "context_fields": {
    "condition": "pneumonia",
    "relevant_time": "discharge",
    "ordered_passages": [
      "Initially treated as possible pneumonia.",
      "Subsequent assessment ruled out pneumonia; symptoms attributed to another condition."
    ],
    "status_definitions": null,
    "rule_results": null
  },
  "missing_fields": [
    "status_definitions",
    "rule_results"
  ],
  "preparation_status": "synthetic_partial_mapping",
  "token_audit": {
    "status": "unverified",
    "tokenizer_revision": null,
    "packed_token_count": null,
    "truncation": null
  },
  "mapping_note": "Editorial extraction from the original synthetic vignette only. Some values are qualitative or underspecified; non-null does not establish decision sufficiency."
}
```

**Missing-information variant:** Remove all information establishing condition from the excerpt and mapped fields. Retrieve the missing fact or hold for review; a future model answer would need the original abstention option. No answer is recorded.

Original cells: `Questions` row 49, `Context_design` same row, `Routing` same row. Background links from the workbook (not evidence of model performance):

- [Background source 1](https://pmc.ncbi.nlm.nih.gov/articles/PMC2757457/)

## Catalogue 1.1.0 extensions

The following additions are proposed tasks with provisional answer definitions. Original entries above are unchanged.

[EC06](#ec06) | [EC07](#ec07) | [EC08](#ec08) | [EC09](#ec09) | [EC10](#ec10) | [EC11](#ec11)

<a id="ec06"></a>

## EC06

**Question:** Does this record meet the surveillance case definition for [condition] during [time window]?

**Decision unit:** One clinical record × surveillance definition × time window

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Meets confirmed-case criteria | Meets the protocol’s confirmed-case criteria; this category takes precedence over probable and suspected categories. |
| 2 | Meets probable-case criteria only | Meets probable-case criteria but not confirmed-case criteria under the supplied hierarchy. |
| 3 | Meets suspected-case criteria only | Meets suspected-case criteria but not probable or confirmed criteria. |
| 4 | Does not meet the case definition | Adequate records establish that the episode fails the operational case definition. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Exact surveillance definition, original passages, symptom onset, diagnostic evidence, negation and subject of the statements. Apply the definition’s hierarchy; if the study does not use these three case categories, replace them.

**Minimum context:** Exact surveillance definition, original passages, symptom onset, diagnostic evidence, negation and subject of the statements. Apply the definition’s hierarchy; if the study does not use these three case categories, replace them.

| Required field | Supplier |
|---|---|
| `case_definition` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `original_passages` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `episode_dates` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `diagnostic_evidence` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `negation_and_subject` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use validated extraction and deterministic rules when all case criteria are explicit and reliably represented.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One clinical record × surveillance definition × time window × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use validated extraction and deterministic rules when all case criteria are explicit and reliably represented.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** A disease mention is not a case. Category names and hierarchy must match the surveillance protocol.

**Proposed validation:** Adjudicated record labels; split by patient, institution and time; measure category recall and case-count bias.

<a id="ec06-synthetic-example"></a>

### EC06 synthetic example

SYNTHETIC: Record R describes fever and cough beginning on day 3. The supplied summary contains no laboratory result and no surveillance case definition.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [WHO event-based surveillance](https://www.who.int/publications/i/item/WHO-HSE-GCR-LYO-2014.4)

<a id="ec07"></a>

## EC07

**Question:** Do these reports support a suspected outbreak in [location] during [time window] under the surveillance protocol?

**Decision unit:** One location × time window × outbreak-signal definition

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supported | The evidence satisfies the protocol’s criteria for a suspected outbreak signal. |
| 2 | Not supported | Adequate evidence does not satisfy the stated signal criteria; this does not exclude an outbreak. |
| 3 | Conflicting evidence | Comparable, usable evidence supports incompatible assessments under the same criteria. |
| 4 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Deduplicated reports, dated case summaries, baseline incidence, operational alert criteria, ascertainment changes and any computed excess or cluster statistics.

**Minimum context:** Deduplicated reports, dated case summaries, baseline incidence, operational alert criteria, ascertainment changes and any computed excess or cluster statistics.

| Required field | Supplier |
|---|---|
| `location` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `time_window` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `deduplicated_reports` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `baseline_summary` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `alert_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `ascertainment_changes` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Apply alert thresholds directly when structured surveillance measures fully determine the signal.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One location × time window × outbreak-signal definition × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Apply alert thresholds directly when structured surveillance measures fully determine the signal.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** A supported signal is not a confirmed outbreak or a calibrated forecast probability.

**Proposed validation:** Expert-adjudicated signals with geographic and temporal holdouts; measure detection delay, false alerts and missed events.

<a id="ec07-synthetic-example"></a>

### EC07 synthetic example

SYNTHETIC: Three deduplicated reports describe a gastrointestinal syndrome in location L during week W. Baseline incidence and the alert protocol are not supplied.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [WHO event-based surveillance](https://www.who.int/publications/i/item/WHO-HSE-GCR-LYO-2014.4)

<a id="ec08"></a>

## EC08

**Question:** Is the documented increase in testing a supported explanation for the excess cases in this mapped cluster?

**Decision unit:** One mapped case cluster × testing-change explanation

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supported as a sufficient explanation | Supplied analyses and contextual evidence support the named factor accounting for the full observed difference under prespecified sufficiency criteria. |
| 2 | Supported as a partial explanation only | Evidence supports a contribution, but also establishes that the named factor alone is insufficient under those criteria. |
| 3 | Evidence argues against this explanation | Adequate evidence is inconsistent with the named factor explaining the observed difference. |
| 4 | Conflicting evidence | Comparable evidence supports incompatible conclusions about the named factor. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Cluster statistics, population denominators, testing volumes, positivity, case definitions and testing-policy dates, with criteria for sufficiency.

**Minimum context:** Cluster statistics, population denominators, testing volumes, positivity, case definitions and testing-policy dates, with criteria for sufficiency.

| Required field | Supplier |
|---|---|
| `cluster_summary` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `population_denominators` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `testing_and_positivity` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `testing_policy_dates` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `sufficiency_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use quantitative decomposition directly when the supplied structured analysis resolves the explanation.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One mapped case cluster × testing-change explanation × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use quantitative decomposition directly when the supplied structured analysis resolves the explanation.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** Testing changes and transmission changes can coexist. Do not infer causality or compute cluster significance from narrative evidence alone.

**Proposed validation:** Adjudicated cluster dossiers with testing changes; compare with spatial-statistical baselines and audit incorrectly dismissed signals.

<a id="ec08-synthetic-example"></a>

### EC08 synthetic example

SYNTHETIC: Case counts and testing volume both increased in district D after a new testing site opened. Positivity and population denominators are missing.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [WHO event-based surveillance](https://www.who.int/publications/i/item/WHO-HSE-GCR-LYO-2014.4)

<a id="ec09"></a>

## EC09

**Question:** Does the supplied evidence support assuming that immunity persists throughout this SEIR simulation period?

**Decision unit:** One immunity-duration assumption × SEIR simulation period

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supported over the specified period | Relevant evidence supports persistence of immunity for the specified population and simulation horizon. |
| 2 | Contradicted by documented waning or reinfection | Relevant evidence documents waning or reinfection incompatible with the stated persistence assumption. |
| 3 | Conflicting evidence | Comparable studies provide incompatible support for the assumption. |
| 4 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Pathogen, population, simulation horizon, immunity assumptions and relevant evidence. A corresponding SEIRS question can evaluate the assumed return to susceptibility.

**Minimum context:** Pathogen, population, simulation horizon, immunity assumptions and relevant evidence. A corresponding SEIRS question can evaluate the assumed return to susceptibility.

| Required field | Supplier |
|---|---|
| `pathogen_and_population` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `simulation_horizon` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `immunity_assumption` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `immunity_evidence` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use established protocol rules when evidence scope and immunity duration settle applicability.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One immunity-duration assumption × SEIR simulation period × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use established protocol rules when evidence scope and immunity duration settle applicability.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** The answer does not estimate parameters, choose a best-fitting model or validate a forecast. Review return-to-susceptibility assumptions separately for SEIRS.

**Proposed validation:** Expert labels for assumption–evidence pairs; independently evaluate model sensitivity and out-of-time forecast performance.

<a id="ec09-synthetic-example"></a>

### EC09 synthetic example

SYNTHETIC: A proposed simulation lasts six months. The supplied immunity study follows participants for only four weeks.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [CDC SEIR outbreak modeling](https://www.cdc.gov/cfa-behind-the-model/php/data-research/local-measles-disease-modeling/index.html)

<a id="ec10"></a>

## EC10

**Question:** Does the evidence support reporting delays as an explanation for the recent difference between observed and predicted case counts?

**Decision unit:** One forecast departure × reporting-delay explanation

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Supported as a sufficient explanation | Supplied analyses and contextual evidence support the named factor accounting for the full observed difference under prespecified sufficiency criteria. |
| 2 | Supported as a partial explanation only | Evidence supports a contribution, but also establishes that the named factor alone is insufficient under those criteria. |
| 3 | Evidence argues against this explanation | Adequate evidence is inconsistent with the named factor explaining the observed difference. |
| 4 | Conflicting evidence | Comparable evidence supports incompatible conclusions about the named factor. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Observed and predicted counts, prediction intervals, reporting-delay distributions, backfill and surveillance notes.

**Minimum context:** Observed and predicted counts, prediction intervals, reporting-delay distributions, backfill and surveillance notes.

| Required field | Supplier |
|---|---|
| `observed_and_predicted_counts` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `prediction_intervals` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `reporting_delay_summary` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `backfill` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `surveillance_notes` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `sufficiency_criteria` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use validated nowcasting and reporting-delay corrections when they fully explain the difference.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One forecast departure × reporting-delay explanation × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use validated nowcasting and reporting-delay corrections when they fully explain the difference.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** A reporting-delay explanation does not establish absence of a transmission change or fix the epidemic model.

**Proposed validation:** Retrospective rolling-origin evaluation using archived data vintages; assess explanation labels and downstream forecast revisions.

<a id="ec10-synthetic-example"></a>

### EC10 synthetic example

SYNTHETIC: Recent reported counts fall below a forecast interval. Reporting logs mention a laboratory backlog; the subsequent backfill is unavailable.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [CDC SEIR outbreak modeling](https://www.cdc.gov/cfa-behind-the-model/php/data-research/local-measles-disease-modeling/index.html)
- [WHO event-based surveillance](https://www.who.int/publications/i/item/WHO-HSE-GCR-LYO-2014.4)

<a id="ec11"></a>

## EC11

**Question:** Does this report document a qualifying exposure relevant to [syndrome] that was known by the forecast cutoff?

**Decision unit:** One exposure report × syndrome definition × forecast cutoff

| Option ID | Label | Proposed operational definition |
|---|---|---|
| 1 | Qualifying exposure documented | An actual qualifying exposure occurred and its report was available by the forecast cutoff. |
| 2 | Related but nonqualifying exposure | An actual exposure is described but fails the supplied relevance, event-time or information-availability criteria. |
| 3 | Qualifying exposure explicitly denied | The relevant exposure is explicitly denied in the reviewed report. |
| 4 | Hypothetical exposure only | Only a possible future or hypothetical exposure is described. |
| 5 | Insufficient evidence | Decisive information is missing, inaccessible or too incomplete to assign another category. This is not a negative finding. |

**Accepted inputs:** Exposure definition, original report, event date, availability date and forecast cutoff.

**Minimum context:** Exposure definition, original report, event date, availability date and forecast cutoff.

| Required field | Supplier |
|---|---|
| `exposure_definition` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `original_report` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `event_date` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `report_availability_date` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `forecast_cutoff` | Researcher-provided protocol, source-linked record or specialist output; record the actual supplier during mapping |
| `source_ids` | Local provenance index |

**Upstream preparation:** Prepare identifiers, timing, QC and relevant quantitative or specialist findings. Use reliable exposure tags and timestamps directly when qualification is unambiguous.

**Optional cached extraction:** Extract reusable source-linked facts and qualifying passages; do not supply a verdict for the same question.

**Reuse key:** One exposure report × syndrome definition × forecast cutoff × source version × protocol version

**Remaining semantic work:** Resolve contextual applicability or narrative relations not already settled by trusted structured fields.

**Bypass Jev/Laya:** Use reliable exposure tags and timestamps directly when qualification is unambiguous.

**Abstain or escalate:** Hold for missing decisive context, out-of-domain input or low reliability; a substantive Insufficient evidence label is distinct from system abstention.

**Combine outside the model:** Combine separately evaluated criteria with a prespecified application rule; preserve conflicts and missingness. Do not multiply unrelated answer probabilities.

**Future routing:** Supported/applicable findings → retain for the next research step; contrary/nonqualifying findings → record the reason, without automatic irreversible exclusion; mixed/conflicting/insufficient/invalid → retrieve evidence or expert review.

**Interpretation limit:** The label is a candidate forecasting input, not proof of predictive gain. Event time alone does not establish information availability.

**Proposed validation:** Adjudicated narratives and timestamp audit; assess incremental forecast performance with temporal holdouts.

<a id="ec11-synthetic-example"></a>

### EC11 synthetic example

SYNTHETIC: An exposure occurred on day 1 but was first reported on day 8. The forecast cutoff is day 5.

This is an unmapped, incomplete source vignette. The JSON context retains required-field gaps. No answer or model output is provided.

**Provenance:** Author-requested extension, REV-009, catalogue 1.1.0; definitions require domain review. Not transcribed from the archival workbook.

Background references, inspected 2026-09-28; not validation of this task:
- [WHO event-based surveillance](https://www.who.int/publications/i/item/WHO-HSE-GCR-LYO-2014.4)
- [scikit-learn data leakage and feature selection](https://scikit-learn.org/stable/common_pitfalls.html)
