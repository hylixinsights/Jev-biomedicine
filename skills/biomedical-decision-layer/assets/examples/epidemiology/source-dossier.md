# Synthetic source dossier

These fictional notes contain no real participant information.

## E1 — Admission note, episode X1
Renal disease had been under follow-up for years. During this admission, renal function deteriorated and dialysis was started.

## E2 — Household note, episode X1
The participant lives with her mother, who has diabetes. The participant reports that her own screening was normal.

## E3 — Ordered respiratory notes, episode X1
At admission, the participant was treated as possible pneumonia. At discharge, the final assessment explicitly ruled out pneumonia and attributed symptoms to another condition.

## E4 — Conflicting discharge addendum, episode X1
A separate discharge addendum records “pneumonia confirmed.” It has the same discharge time anchor and neither entry supersedes the other.

## Rule summary
A simple temporal rule detects “for years” before “this admission” in E1. A role parser resolves mother as the experiencer of diabetes in E2. A status parser finds possible at admission and ruled out at discharge in E3. These are supplied synthetic rule outputs, not software executed for this repository.

## Missing-source variant
Only “renal function deteriorated and dialysis was started” is available, with no prior history or episode anchor. Missing history cannot become evidence of new onset.
