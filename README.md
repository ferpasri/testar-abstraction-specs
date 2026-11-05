# TESTAR Abstraction Specs

Shared abstraction language, behavior, and conformance cases for constructing abstract models from concrete testing evidence.

Abstraction rules select which recorded state, widget, and action properties matter for comparison. Matching observations form abstract states; matching available actions and executions form abstract actions. Completed recorded transitions support abstract transitions. The model retains references to the original evidence.

Implementations follow the same abstraction rules and verify their results against shared conformance cases.

## Repository contents

- [Terminology](docs/terminology.md): abstraction, State Models, comparisons, and supporting records.
- [Specification](docs/SPEC.md): inputs, matching behavior, supporting evidence, and processing limits.
- [Abstraction language](docs/language.md): rule fields and normalization operations.
- [Examples](docs/examples.md): policies illustrating different testing objectives.
- [Conformance cases](conformance/README.md): generic input, rules, and expected results.
- [Compatibility and releases](docs/versioning.md): shared identifiers and release preparation.
- [TODO](TODO.md): remaining specification and case work.

The [conformance manifest](conformance/manifest.json) records supported format identifiers and lists every shared case. Evidence terminology and concrete trace formats come from [TESTAR Evidence Specs](https://github.com/ferpasri/testar-evidence-specs).

## Use the shared cases

The conformance cases contain language-independent inputs, rules, and expected results. Implementations obtain them from a published release, apply the rules, and compare the resulting memberships and relationships with the expectations. See the [case guide](conformance/README.md) for the input and comparison steps.
