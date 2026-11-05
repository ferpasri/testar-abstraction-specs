# Abstraction terminology

This repository defines terms for deriving models from recorded evidence. Recorded terms such as observation, recorded state, widget, property, available action, and action execution come from [Evidence terminology](https://github.com/ferpasri/testar-evidence-specs/blob/main/docs/terminology.md). Execution statuses and lifecycle come from the [evidence format](https://github.com/ferpasri/testar-evidence-specs/blob/main/docs/evidence-format.md#lifecycle-and-validation).

## Models and abstraction

| Term | Meaning |
| --- | --- |
| Abstraction rules | Decide which captured differences matter when grouping observations and actions. |
| State Model / abstract model | A model derived from traces using abstraction rules. It contains abstract states, actions, and transitions, retaining references to the original evidence. |
| Abstract state | Observations grouped according to the selected state and widget comparisons. |
| Abstract action | Available actions or executions grouped according to selected action comparisons within an abstract source state. |
| Abstract transition | A relationship from a source abstract state, through an abstract action, to a target abstract state, supported by completed recorded transitions. |
| Source abstract state | The abstract state from which an action or transition starts. |
| Target abstract state | The abstract state reached by a transition. |

Use qualified terms: a recorded state is captured evidence; an abstract state is a grouping produced by abstraction. Likewise, an action description, available action, and action execution retain their evidence meanings when used to build abstract actions.

Different rules can produce different models from the same traces.

## Changing application behavior

| Term | Meaning |
| --- | --- |
| Dynamism | Application data or widgets change over time. Abstraction rules determine whether captured changes produce different abstract states or actions, or remain grouped together. |
| Multiple observed outcomes | Recorded executions matching the same abstract source state and abstract action have reached different target abstract states. |
| Non-determinism | An action can produce different outcomes even when its relevant starting conditions and inputs are the same. Matching abstract states and actions alone does not establish that those conditions and inputs were the same. |

Multiple observed outcomes are evidence about the derived model. They can reflect application non-determinism or differences that the abstraction does not distinguish. For example, action grouping may ignore input values, even though those values affect the result. The model retains the observed outcomes and their supporting records without automatically classifying the application as non-deterministic.

The ASM uses this information to decide how to navigate, retry, or explore elsewhere. Those decisions belong to the [ASM exploration controller](https://github.com/ferpasri/testar-state-model-asm/blob/main/docs/exploration-controller.md).

## Comparison

| Term | Meaning |
| --- | --- |
| Comparison field | A named selection of captured properties used to compare records. |
| Normalization | Transform selected comparison values before comparing them, while preserving original recorded values. |
| Widget correspondence | Widget references whose selected descriptions match within an observation group. |
| Ambiguous comparison | The selected properties do not distinguish multiple available actions or widgets in one observation. |
| Unmatched record | An observation, available action, or execution that cannot be compared under the selected rules, retained with an explanation. |

A property alias identifies another name for the same captured property within one record; its definition belongs to [Evidence terminology](https://github.com/ferpasri/testar-evidence-specs/blob/main/docs/terminology.md#capture-and-available-actions). Cross-record matching uses the explicit abstraction rules.

The [language](language.md) defines comparison fields and transforms. The [specification](SPEC.md) defines grouping and action matching.

## Model links and support

| Term | Meaning |
| --- | --- |
| Supporting reference | A reference to an original source, run, observation, widget, or execution supporting a model element or comparison. |
| Recorded action relationship | An execution uses the same observation, action description, and target widget as a reported available action. |
| Abstraction action relationship | Selected comparison values match an execution to an abstract action without that recorded availability relationship. |
| Observation-order link | A connection between consecutive observations in one test sequence when no completed recorded transition connects them. It expresses recorded order, not an executed action. |
| Source fingerprint | A content identifier for a supplied evidence collection, used to check that model construction refers to the same input. |

An observation-order link does not supply an action for navigation. A path through an abstract model can also combine support from different traces; it does not establish that one test sequence followed the entire path.

Action selection, unvisited actions, and navigation objectives are defined by [ASM terminology](https://github.com/ferpasri/testar-state-model-asm/blob/main/docs/terminology.md). Abstraction supplies the model and its supporting references; ASMs decide how to use it.
