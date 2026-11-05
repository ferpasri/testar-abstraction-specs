# TODO

- [ ] Agree on Unicode-version boundaries and add casing cases for characters whose behavior differs between supported runtimes.
- [ ] Define numeric precision and fixture-reading requirements for large integers and decimals.
- [ ] Extend URL canonicalization cases for host names, default ports, and query encoding across languages.
- [ ] Define portable error categories and expectations for rejected definitions and processing limits.
- [ ] Specify the generic boundary for matching a current observation and available actions alongside historical evidence.
- [ ] Review identity and canonicalization requirements when consumers exchange generated model references.
- [ ] Define a portable abstract-model exchange format with abstraction rules, behavior identifiers, selected evidence, and supporting references.
- [ ] Extend shared model cases for rule identity/configuration, selected input scope, and supporting evidence references.
- [ ] Specify model rebuilding and incremental updates for consumers.
- [ ] Add quality checks for chosen abstractions according to their testing objectives, separately from rule-interpreter conformance.

## Defined responsibilities

- [x] Define abstraction rules, State Models, comparisons, and supporting relationships in the [terminology](docs/terminology.md), referencing recorded terms from Evidence Specs.
- [x] Configure state and action comparisons separately while retaining recorded-transition support.
- [x] Define matching and grouping rules with references to the original evidence.
