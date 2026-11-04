# Abstraction conformance cases

Each case contains three files:

| File            | Meaning                                                                                                                                    |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `input.json`    | Reconstructed evidence records and the selected test runs, with record collections represented as arrays.           |
| `rules.json`    | The abstraction rules applied to that evidence.                                                                                            |
| `expected.json` | The expected observation groups, transition support, order links, unmatched observations, unlinked executions, and widget correspondences. |

`input.json` contains the evidence after an evidence reader has reconstructed a trace. Applying `rules.json` produces an abstract model. `expected.json` describes the memberships and relationships that model must contain.

To run a case:

1. Read the reconstructed records and selected runs from `input.json`.
2. Apply the supplied abstraction rules.
3. Compare the resulting groups and supporting records with `expected.json`.
4. Repeat with source and record collection order reversed, and verify that the input records remain unchanged.

Compare group memberships and support collections independently of their listing order. Widget order follows the supplied rules. Expected results use original-record references to identify the evidence supporting each relationship.

The [manifest](manifest.json) lists every case and its compatibility identifiers.

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


## Action cases

`action-cases/` uses the same input and rules files. Its expected result describes abstract actions, availability references and scopes, execution references and statuses, recorded versus inferred relationships, ambiguity, and transition support. Source-state memberships identify the states in the expected result. Diagnostic fields indicate whether an explanation is required.

| Case                        | Behavior                                                                                                                                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `available-and-executed`    | Report A1 through A5, then link A4 from a later observation and A5 from another source, with different recorded IDs and an explicitly normalized operation. |
| `ambiguous-targets`         | Retain a recorded link to one of two identical button descriptions; leave a cross-source execution unmatched.                                               |
| `missing-targets`           | Keep available actions and executions with missing target information visible as unmatched records.                                                         |
| `selected-parameters`       | Match a chosen planned input value against an executed input value.                                                                                         |
| `recorded-input-difference` | Retain a recorded selection link while grouping the execution by its actual input values, with a diagnostic.                                                |
| `execution-statuses`        | Preserve completed, failed, incomplete, and awaiting-result statuses; only a completed execution supports a transition.                                     |
| `actions-scoped-by-state`   | Keep the same action description separate when it belongs to different abstract states.                                                                     |

For action cases, also reverse the selected runs when checking order independence.

## Loan visibility example

The three `loan-*` cases apply different rules to identical evidence: structure alone groups all observations, adding captured styles separates form and result, and adding result text separates the two results. All executions retain their original support. See the [worked example](../docs/examples.md#captured-styles-and-result-text) for the property distinctions.
