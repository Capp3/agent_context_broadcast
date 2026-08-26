# Research Manifest

This manifest tracks standalone report status, source access, and regeneration policy. It intentionally avoids consumer-specific fields; downstream projects should maintain their own integration notes.

## Repository Contract

- Durable artifacts: `reports/**/*.md`, `report-template.md`, `manifest.md`, `qa-checklist.md`, and reusable prompt templates.
- Disposable artifacts: one-off prompts used to create prior reports.
- Consumer boundary: consuming repositories own path mapping, import strategy, pinning, and integration tests.
- Standalone boundary: each report must include enough local context to be useful without loading another report, while detailed treatment remains with the owning report.

## Reports

| Report ID | File | Topic | Status | Source access | Prompt template | Regeneration policy |
| --------- | ---- | ----- | ------ | ------------- | --------------- | ------------------- |
| `st2110-overview` | `reports/st2110/overview.md` | ST 2110 overview and cross-domain context | Draft converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when two or more owning domain reports materially change. |
| `st2110-transport-and-essence` | `reports/st2110/transport-and-essence.md` | ST 2110 transport and essence | Draft converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when SMPTE part versions, public excerpts, or licensed extracts change. |
| `st2110-bandwidth-and-capacity` | `reports/st2110/bandwidth-and-capacity.md` | ST 2110 bandwidth and capacity calculations | Draft converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when formulas, pgroup tables, packetization constraints, worked examples, or evidence labels change. |
| `aes67` | `reports/audio/aes67.md` | AES67 IP audio interoperability | Draft converted reference | Mixed public, preview, and secondary | `template-convert-existing-context.md` | Regenerate when AES67 preview/full text, PICS material, or dependent RFC versions change. |
| `ptp-timing-and-synchronization` | `reports/timing/ptp.md` | PTP timing and synchronization | Draft converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when IEEE 1588, ST 2059, RTP clock signaling, or timing evidence sources change. |
| `ttml-imsc` | `reports/timed-text/ttml-imsc.md` | TTML and IMSC timed text | Draft converted reference | Public | `template-convert-existing-context.md` | Regenerate when W3C Recommendations, Candidate Recommendations, or errata change. |

## Known Gaps

| Gap | Status | Resolution |
| --- | ------ | ---------- |
| Dedicated NMOS report | Open | Create a standalone NMOS report with `template-new-topic.md` or keep NMOS scoped to the master synthesis until needed. |
| Full frontmatter normalization | Closed | Required frontmatter was added during repository reorganization on 2026-08-26. |
| Licensed standards verification | Open | Reports must keep public/secondary limitations visible until licensed standards text is attached and cited. |
| Related report metadata | Open | Decide whether `related_reports` should become required frontmatter or remain a body-section convention. |

## Versioning Policy

- Use semantic versions for report content: `major.minor.patch`.
- Increment major when section contracts or conclusions change incompatibly.
- Increment minor when new standards, formulas, examples, or validation rules are added.
- Increment patch for citation fixes, typo fixes, and non-behavioral cleanup.
- Record superseded report IDs in frontmatter when replacing a report.

## Regeneration Policy

Regenerate or reconvert a report when:

- A primary standard, profile, RFC, schema, or Recommendation changes.
- A source conflict is resolved by an authoritative source.
- A formula, constant, table, or worked example is found to be wrong.
- A report contains project-specific language.
- A downstream consumer needs a stable section that is absent from the report.

Use `prompts/template-routine-update.md` before full regeneration when the trigger is a source change, public documentation release, errata notice, standards-body announcement, or official repository update. Prefer a controlled patch when only citations, source status, unverified items, formulas, or specific requirements are affected.

## Routine Update Policy

| Trigger | Prompt | Expected result |
| ------- | ------ | --------------- |
| New public source access | `template-routine-update.md` | Source delta register and evidence promotion/downgrade proposal. |
| Official errata or amendment | `template-routine-update.md` | Impact assessment and controlled patch. |
| Formula or worked example issue | `template-routine-update.md` | Recalculated example and formula impact register. |
| Major topic expansion | `template-new-topic.md` or `template-convert-existing-context.md` | New report or full reconversion. |
| Report structure drift | `template-convert-existing-context.md` | Normalized standalone report. |

## Prompt Disposal Policy

One-off prompts are not preserved as durable artifacts. A prompt may remain only if it is reusable across future topics or conversions and is documented in `readme.md`.
