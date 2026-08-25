# QA Checklist

Use this checklist before marking a report as `reviewed` in `manifest.md`.

## Scope And Independence

- [ ] The report title is standalone.
- [ ] The report has no product-specific, repo-specific, roadmap, or chat-context references.
- [ ] Consumer integration details are absent from the report.
- [ ] The report explains its own scope and boundaries.

## Source And Citation Review

- [ ] Every substantive technical claim has an inline citation or an explicit evidence label.
- [ ] Each source has version/date, role, source status, and clause-level visibility in the Standards and Source Map.
- [ ] Primary sources are separated from secondary sources.
- [ ] Paywalled, partial, preview-only, draft, or inaccessible sources are labeled.
- [ ] Source conflicts are preserved rather than silently reconciled.

## Normative Language Review

- [ ] Every `must`, `shall`, `required`, `prohibited`, or `forbidden` statement maps to a normative citation.
- [ ] Implementation requirements are labeled as implementation guidance, not standards requirements.
- [ ] Best practices are not presented as normative requirements.
- [ ] Assumptions are labeled and justified.
- [ ] Unverified claims remain useful but are clearly marked.

## Formula And Example Review

- [ ] Every formula includes name, source, inputs, units, output units, and confidence.
- [ ] Integer, rounding, rate, precision, clock, and packetization behavior is explicit where relevant.
- [ ] Worked examples show intermediate values.
- [ ] Worked examples recalculate correctly.
- [ ] Constants and lookup tables have citations.
- [ ] Vendor estimates and rules of thumb are labeled non-normative.

## Table Review

- [ ] Standards and Source Map is present.
- [ ] Normative Requirements Catalog is present when the topic includes requirements.
- [ ] Formula and Assumption Register is present when the topic includes calculations.
- [ ] Risk and Ambiguity Register is present.
- [ ] Tables use `Unverified` or `Not provided in input` rather than fabricated cells.

## Diagram Review

- [ ] Mermaid diagrams render.
- [ ] Diagram labels do not introduce unsupported claims.
- [ ] Diagram IDs avoid spaces, reserved words, and styling directives.
- [ ] Diagrams clarify relationships already described in cited text.

## Report Contract Review

- [ ] Frontmatter matches `report-template.md`.
- [ ] Required section order is followed or deviation is documented in `manifest.md`.
- [ ] Evidence labels match `report-template.md`.
- [ ] Open questions are actionable.
- [ ] Sources are complete enough for future regeneration.

## Final Checks

- [ ] Search for project-specific names and remove any remaining matches.
- [ ] Search for old prompt filenames and remove references unless discussing prompt disposal policy.
- [ ] Update `manifest.md` status, version, source access, and regeneration policy.
- [ ] Record any known gap rather than hiding it in prose.
