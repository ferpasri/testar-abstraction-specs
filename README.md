# TESTAR Abstraction Specs

Shared abstraction language, behavior, and conformance cases for constructing abstract models from concrete testing evidence.

Abstraction rules select which recorded state, widget, and action properties matter for comparison. Matching observations form abstract states; matching available actions and executions form abstract actions. Completed recorded transitions support abstract transitions. The model retains references to the original evidence.

Implementations in different languages follow the same specification and cases. The TypeScript implementation serves browser and Node.js consumers. Native Java and Python implementations can provide abstraction to their corresponding ASM libraries.

## Repository contents

- [Specification](docs/SPEC.md): inputs, matching behavior, supporting evidence, and processing limits.
- [Abstraction language](docs/language.md): rule fields and normalization operations.
- [Examples](docs/examples.md): policies illustrating different testing objectives.
- [Conformance cases](conformance/README.md): generic input, rules, and expected results.
- [Compatibility and releases](docs/versioning.md): shared identifiers and release preparation.
- [TODO](TODO.md): remaining specification and case work.

The [conformance manifest](conformance/manifest.json) records supported format identifiers and lists every shared case. Evidence terminology and concrete trace formats come from [TESTAR Evidence Specs](https://github.com/ferpasri/testar-evidence-specs).

## Use the shared cases

Implementations obtain the cases from a published GitHub release of this repository. They reconstruct the generic inputs, apply the supplied rules, and compare their results with the expected memberships and relationships. Original evidence references are used for comparison; generated model IDs and display labels are implementation details.

The cases are data files that any language can read. They do not require a JavaScript runtime or a dependency on the TypeScript implementation.
