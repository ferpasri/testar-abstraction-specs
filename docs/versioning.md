# Compatibility and releases

Published GitHub tags identify immutable revisions of the shared specification and cases. Implementations record the shared release they verify and the corresponding archive digest. A documentation change does not by itself change the meaning of rules.

## Shared identifiers

The [conformance manifest](../conformance/manifest.json) is the single source for:

| Field | Meaning |
| --- | --- |
| `case_format_version` | Structure used to list the suites and interpret their fixture files. |
| `evidence_schema` | Concrete evidence schema from which the reconstructed input records originate. |
| `rule_language` | Identifier required in abstraction rule definitions. |
| `behavior_version` | Interpretation behavior that implementations reproduce. |
| `suites` | State and action case directories and complete case names. |

The specs release, an implementation package version, the rule-language identifier, and interpretation behavior identify different things. The first two package version components follow the TESTAR Evidence compatibility family; package patches can advance independently when their declared behavior stays compatible.

A new release can add documentation or cases while retaining the existing language and behavior identifiers. A change to the meaning of existing rules requires an explicit compatibility decision and corresponding case updates.

## Prepare a release

1. Verify that every listed case contains valid `input.json`, `rules.json`, and `expected.json` files.
2. Verify the cases against the affected implementations and update compatibility identifiers when behavior changes.
3. Commit and push the reviewed specification and data.
4. Create a GitHub release and tag for that revision.

Consumers use the published release archive. These documents and data files need no language-specific build or runtime.
