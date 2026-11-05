# Abstraction specification

The [terminology](terminology.md) defines derived model concepts. Recorded observations, states, actions, and execution lifecycle follow [Evidence Specs](https://github.com/ferpasri/testar-evidence-specs/blob/main/docs/terminology.md).

## Inputs

A caller supplies reconstructed evidence sources, selected test runs, and abstraction rules. An evidence reader validates the original trace format and execution lifecycle before supplying these records.

Each source has an identity, display label, and fingerprint identifying its content. Record references include their source and, where appropriate, test run or observation, so records remain associated with the evidence that supplied them.

The input contains collections of test runs, test sequences, observations, action descriptions, and action executions. Observations retain recorded states, widgets, capture information, available actions, and artifact references. Execution records retain their reported status and any resulting observation. Abstraction preserves the supplied records.

## Group observations

Apply the rules to the selected evidence and compare the chosen state and widget properties. Return the observation groups, compared values, widget correspondences, and reasons for unmatched observations.

Normalization changes comparison values only. Original properties and addresses remain available in the evidence. Missing properties, JSON null, and unsuitable transform inputs have distinct meanings. Capture status participates when `capture` is `same`.

Repeated widget descriptions retain their original references and ambiguity. Correspondences express matches under the selected rules.

## Construct an abstract model

Verify that grouping results describe the supplied source fingerprints and selected test runs before constructing the model. Every observation remains represented, including unmatched observations with their explanations.

Completed executions support abstract transitions when their source and resulting observations are available. With action rules, executions sharing endpoint states and the same abstract action share a transition. An execution that cannot be matched keeps separate transition support. Without action rules, endpoint states and the exact recorded action type determine grouping. Every supporting execution remains available.

Failed, incomplete, and otherwise unconnected executions retain their reported status and explanation. An observation-order link can connect consecutive observations in one sequence when no completed execution connects them. It records observation order separately from executed transitions.

Grouping and model construction are deterministic for the same evidence and rules, independent of source order. Results retain the applied rules, interpretation behavior identifier, source fingerprints, and selected test runs.

## Match available actions and executions

Optional action rules select operation normalization, action-description properties, target-widget properties, and parameters. Equal selected values produce one abstract action within its source abstract state. The [language reference](language.md#match-actions) defines these selections.

Each abstract action retains its compared values, source state, availability records with their reported scope, and supporting executions with their original status. References identify the source, run, and relevant observation, action description, widget, or execution. Unmatched available actions and executions retain reasons.

A recorded relationship means an execution uses the same observation, action description, and target widget as a reported available action. Retain this link even if selected input values differ. The execution then belongs to the abstract action described by its actual values, with an explanation of the difference. If it cannot be compared, keep it unmatched with its recorded availability references. An abstraction relationship means selected comparison values establish the match without that recorded availability link. Executions can also establish an action that was never reported as available.

When distinct available actions in one observation have equal comparison values, mark the abstract action as ambiguous. Recorded execution links remain usable. Other executions that would match that group stay unmatched and retain references to the possible abstract actions. Identical repeated availability records count once.

An action without supporting executions means no execution matched in the selected evidence. Failed and unfinished attempts retain their status. Availability records identify the source that reported each action, and oracle results remain attached to their original targets.

## Processing limits

| Input or processing resource | Limit |
| --- | ---: |
| Rule definition, UTF-8 | 64 KiB |
| Selected observations | 10,000 |
| Selected widgets | 100,000 |
| Widget hierarchy depth | 128 |
| Normalized value, UTF-8 | 1 MiB |
| Accumulated comparison data per state or action pass | 32 MiB |
| Availability and execution records combined for action comparison | 100,000 |

Exceeding a processing limit produces an error. An observation or action with unsuitable comparison values remains unmatched with a reason.

## Shared verification

The [conformance cases](../conformance/README.md) compare observation memberships, action matches, and their supporting evidence. Equivalent inputs and rules produce equivalent relationships across implementations. The cases start from reconstructed records; evidence-format validation is covered by TESTAR Evidence Specs.

Shared identifiers and release preparation are described in [compatibility and releases](versioning.md).
