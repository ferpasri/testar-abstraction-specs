# Abstraction conformance cases

Each case contains three files:

| File            | Meaning                                                                                                                                    |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `input.json`    | Generic reconstructed evidence records and the selected test runs. Maps are represented as arrays so any language can read them.           |
| `rules.json`    | The abstraction rules applied to that evidence.                                                                                            |
| `expected.json` | The expected observation groups, transition support, order links, unmatched observations, unlinked executions, and widget correspondences. |

These inputs describe records after an evidence reader has reconstructed a trace. They are separate from the original JSONL evidence-format conformance cases in `testar-evidence-specs`.

To run a case, turn each record array into a map keyed by its ID (`execution.id` for execution records), apply the rules to the selected runs, and compare the result with `expected.json`. Array order is ignored. Expected memberships use original-record references rather than implementation-generated group IDs or display labels.

| Case                    | Behavior                                                                                                             |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `normalized-pages`      | Merge pages after explicit session-data removal and title normalization; retain execution support from both sources. |
| `filtered-widgets`      | Exclude a changing widget while retaining correspondence between stable widgets.                                     |
| `unmatched-and-order`   | Retain a missing-title observation, failed/incomplete executions, and distinct observation-order links.              |
| `capture-same`          | Keep different reported capture statuses separate.                                                                   |
| `loan-structure-only`   | Group form, denied result, and approved result when styles and text are not selected.                                |
| `loan-display-changes`  | Separate the form from the result panel using captured styles; denied and approved results still match.              |
| `loan-display-and-text` | Also compare result text to distinguish denied and approved results at the same address and title.                   |
| `capture-ignore`        | Group the same observations when the rules explicitly ignore capture status.                                         |

The [manifest](manifest.json) lists the state and action suites and their compatibility identifiers. Each implementation runs these cases through its own public API. Verification also reverses source order and confirms that supplied records remain unchanged.

## Action cases

`action-cases/` uses the same input and rules files. Its expected result describes abstract actions, availability references and scopes, execution references and statuses, recorded versus inferred relationships, ambiguity, and transition support. Source-state memberships replace generated state IDs. Diagnostics are checked for presence; generated abstract-action IDs and wording are not part of the portable expected result.

| Case                        | Behavior                                                                                                                                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `available-and-executed`    | Report A1 through A5, then link A4 from a later observation and A5 from another source, with different recorded IDs and an explicitly normalized operation. |
| `ambiguous-targets`         | Retain a recorded link to one of two identical button descriptions; leave a cross-source execution unmatched.                                               |
| `missing-targets`           | Keep available actions and executions with missing target information visible as unmatched records.                                                         |
| `selected-parameters`       | Match a chosen planned input value against an executed input value.                                                                                         |
| `recorded-input-difference` | Retain a recorded selection link while grouping the execution by its actual input values, with a diagnostic.                                                |
| `execution-statuses`        | Preserve completed, failed, incomplete, and awaiting-result statuses; only a completed execution supports a transition.                                     |
| `actions-scoped-by-state`   | Keep the same action description separate when it belongs to different abstract states.                                                                     |

Action verification also reverses sources, selected runs, and record collection order, and confirms that original records remain unchanged.

## Loan visibility example

The three `loan-*` cases apply different rules to identical evidence: structure alone groups all observations, adding captured styles separates form and result, and adding result text separates the two results. All executions retain their original support. See the [worked example](../docs/examples.md#captured-styles-and-result-text) for the property distinctions.

## Reading a case

`input.json` is the reconstructed evidence supplied to abstraction; it is not an expected model and is not a raw JSONL trace. `rules.json` specifies how to compare those records. The implementation produces an abstract model, and `expected.json` describes the memberships and relationships that model must contain.

Each listed case requires all three files. State cases and action cases have different expected-result projections, described above. Implementations obtain the files from this repository's release archive and record the selected release and archive digest. Shared fixtures are maintained here rather than copied into language repositories.
