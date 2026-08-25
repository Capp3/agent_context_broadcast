# Research Manifest

This manifest tracks standalone report status, source access, and regeneration policy. It intentionally avoids consumer-specific fields; downstream projects should maintain their own integration notes.

## Repository Contract

- Durable artifacts: `report-*.md`, `report-template.md`, `manifest.md`, `qa-checklist.md`, and reusable prompt templates.
- Disposable artifacts: one-off prompts used to create prior reports.
- Consumer boundary: consuming repositories own path mapping, import strategy, pinning, and integration tests.

## Reports

| Report ID | File | Topic | Status | Source access | Prompt template | Regeneration policy |
| --------- | ---- | ----- | ------ | ------------- | --------------- | ------------------- |
| `master` | `report-master.md` | Cross-domain ST 2110, PTP, bandwidth, and NMOS synthesis | Converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when two or more domain reports materially change. |
| `st2110` | `report-st2110.md` | ST 2110 transport and essence | Converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when SMPTE part versions, public excerpts, or licensed extracts change. |
| `ptp` | `report-ptp.md` | PTP timing and synchronization | Converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when IEEE 1588, ST 2059, or RTP clock signaling sources change. |
| `bandwidth` | `report-bandwidth.md` | ST 2110 bandwidth and capacity calculations | Converted reference | Mixed public and secondary | `template-convert-existing-context.md` | Regenerate when formulas, pgroup tables, packetization constraints, or worked examples change. |
| `aes67` | `report-aes67.md` | AES67 IP audio interoperability | Converted reference | Mixed public, preview, and secondary | `template-convert-existing-context.md` | Regenerate when AES67 preview/full text, PICS material, or dependent RFC versions change. |
| `ttml` | `report-ttml.md` | TTML and IMSC timed text | Converted reference | Public | `template-convert-existing-context.md` | Regenerate when W3C Recommendations, Candidate Recommendations, or errata change. |

## Known Gaps

| Gap | Status | Resolution |
| --- | ------ | ---------- |
| Dedicated NMOS report | Open | Create a standalone NMOS report with `template-new-topic.md` or keep NMOS scoped to the master synthesis until needed. |
| Full frontmatter normalization | Open | Add required frontmatter during the next report regeneration or conversion pass. |
| Licensed standards verification | Open | Reports must keep public/secondary limitations visible until licensed standards text is attached and cited. |

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
