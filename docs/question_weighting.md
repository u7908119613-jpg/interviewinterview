# Question Weighting

After each interviewer perspective generates questions, map them to the generic question bank and weight them by cross-interviewer overlap. The goal is simple: questions that more than one interviewer is likely to raise should be rehearsed first.

---

## Step 1: Map to the question bank

For each generated question, compare against [templates/question_bank_template.md](../templates/question_bank_template.md) (categories E / W / P / S):

- **Hit** — semantically matches a bank category/item → map to that `question_id` (e.g. `E1`, `W2`).
- **Variant** — same direction, different angle → map to the nearest item, mark `variant`.
- **Novel** — no bank match → tag `FREE-XX` (interviewer/committee-specific).

## Step 2: Cross-interviewer weighting

Track which interviewer raised each question in an `asked_by` field.

- +1 requires the same question (or mapped `question_id`) raised by **2 or more source-backed (real, known) interviewers** — lead and/or known panel.
- **Inferred perspectives `(inferred, no source)` never count toward this threshold.** A lead + 1 inferred, or 2 inferred, does NOT promote — inferred lenses are derived from the same project text and carry no real-person signal. Inferred overlap may be *noted* in `asked_by` (e.g. "also raised by inferred GIS lens") but does not change priority. This keeps the rule's letter aligned with its intent: only concern shared across real committee members earns earlier rehearsal.
- **FREE-XX dedup**: two interviewers may raise semantically-equivalent *novel* questions that would get different `FREE-XX` IDs and never match. Before assigning IDs, dedupe FREE questions semantically across interviewers; merge equivalents into one `FREE-XX` with both in `asked_by`, then apply +1. Committee-shared novel concerns are high-value — don't let them fall through.
- **Single-interviewer runs** (`pi=` only): nothing reaches 2 interviewers, so weighting is a no-op; ordering falls back to category order with the sole PI's questions first (Step 3).
- Priority is capped at the top tier. Rationale: a concern shared across real committee members is more likely to come up and more costly to fumble.

## Step 3: Ordering

Within the prep page (Phase 3) and the companion practice loop (Phase 4), order questions by:

1. priority (cross-interviewer overlap, highest first)
2. lead-interviewer questions
3. panel probes
4. `FREE-XX` (interviewer/committee-specific)

## Output fields

Record per question:

```text
question_id | text | category (E/W/P/S/FREE) | asked_by (which interviewers) | priority
```

Example (fictional):

| question_id | asked_by | priority | note |
|---|---|---|---|
| E3 | lead + known panel (stats) | high | 2 real interviewers → +1 |
| FREE-01 | lead + inferred GIS lens | normal | only 1 real → no +1 (inferred noted, not counted) |
| P1 | lead | normal | single source |
| FREE-02 | inferred methodology lens | normal | inferred-only, panel-specific |

`FREE-XX` items are candidate new bank entries. In this public version they stay local to the run — there is no shared question bank to write back to.
