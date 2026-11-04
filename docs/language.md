# Abstraction language

Rules select captured values for comparing observations and actions. Presets and custom definitions use the same format. See the [examples](examples.md) for complete policies and [compatibility](versioning.md) for the shared identifiers.

## Definition

| Field     | Meaning                                                               |
| --------- | --------------------------------------------------------------------- |
| `version` | Required supported rule-language identifier.                          |
| `name`    | Required readable policy name.                                        |
| `state`   | Required list of comparison fields reading recorded-state properties. |
| `capture` | `same` includes capture status in comparison; `ignore` leaves it out. |
| `widgets` | Optional widget fields, exclusions, hierarchy, and order rules.       |
| `actions` | Optional action-comparison rules within each abstract source state.   |

A definition selects at least one state field or a widget comparison. Unknown structural fields and duplicate comparison-field names are rejected. Definitions follow the [processing limits](SPEC.md#processing-limits).

## Comparison fields

Each field has a `name` and a nonempty `keys` list. Keys are literal property names tried in order; the first property present in the supplied JSON object supplies the value. `path` optionally follows nested keys within that value. `required` rejects a missing value; optional missing values remain distinct from JSON null. `transforms` applies the selected string operations in order.

| Operation             | Behavior                                                                      |
| --------------------- | ----------------------------------------------------------------------------- |
| `trim`                | Remove leading and trailing whitespace.                                       |
| `collapse_whitespace` | Replace each whitespace run with one space.                                   |
| `lowercase`           | Convert text to lowercase.                                                    |
| `replace`             | Replace every literal occurrence of `find` with `with`.                       |
| `map`                 | Replace a string listed in `values`; preserve an unmapped string.             |
| `url`                 | Normalize an absolute URL with the explicitly selected removal options below. |

String transforms reject non-string inputs. An observation with a missing required field, unsuitable value, or no usable comparison values remains unmatched with a reason. The source values remain unchanged.

### URL normalization

`remove_path_parameters` removes named semicolon parameters from every path segment, including parameters without values. Names are exact and case-sensitive. Names are percent-decoded once after literal separators are identified; encoded separators are not treated as boundaries. Malformed encoded names stay unchanged. Plus signs in path-parameter names are literal. Unrelated parameters retain their order and encoding.

`remove_query` removes the selected query names. `remove_fragment: true` removes the fragment. Path parameters, query data, and fragments are retained unless the corresponding option selects them for removal. See the [address example](examples.md#page-addresses-and-titles).

## Widget comparison

`widgets.fields` selects widget properties using comparison fields. `exclude` is a list of predicates reading original values, before field transforms. A predicate has alternative `keys` and tests `present`, `equals` with a JSON `value`, or `contains` with a string `value`. Matching any predicate excludes that widget.

`descendants: "exclude"` also excludes descendants of excluded widgets. `"keep"` retains them and attaches them to the nearest retained ancestor. `hierarchy: "tree"` compares retained parent-child relationships; `"flat"` compares a collection of descriptions. `order: "preserve"` retains captured order; `"sort"` normalizes it. Repeated descriptions retain their multiplicity.

Unavailable widget capture leaves the observation unmatched. Capture status and the recorded description of capture scope retain their separate meanings. Widget correspondences require selected captured values, and repeated descriptions retain their ambiguity. Style comparison uses the captured values; widget filtering follows the explicit exclusion rules. See the [style and text example](examples.md#captured-styles-and-result-text).

## Match actions

The optional `actions` section selects:

| Field                  | Input                                                                                    |
| ---------------------- | ---------------------------------------------------------------------------------------- |
| `type`                 | Transforms for the recorded `action_type`. The normalized operation always participates. |
| `description`          | Comparison fields reading action-description properties.                                 |
| `target`               | Comparison fields reading the bound widget in the source observation.                    |
| `parameters.available` | Comparison fields reading planned values from action-description properties.             |
| `parameters.executed`  | Comparison fields reading actual execution parameters.                                   |

Both parameter lists must use the same field names; their keys and transforms may differ. Each selected field list needs at least one captured value. Missing required fields, an unavailable bound target, or unsuitable values leave the action unmatched with a reason.

An empty `actions` object compares only the operation. When `actions` is omitted, transitions are grouped by their endpoint states and exact recorded action type. Property and operation mappings follow the explicit rules. Recorded IDs identify supporting records; selected comparison values determine cross-source matching.

The [specification](SPEC.md#match-available-actions-and-executions) defines recorded links, inferred matches, and ambiguous results. The [action examples](examples.md#match-actions) demonstrate operation mapping and parameter selection.
