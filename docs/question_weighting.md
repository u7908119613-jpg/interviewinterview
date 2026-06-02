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

- If the same question or mapped `question_id` is raised by **2 or more interviewers**, increase its priority by **+1**.
- **At least one must be source-backed** (a real, known interviewer). Two inferred perspectives `(inferred, no source)` agreeing does NOT grant +1 on its own — they are derived from the same project text and carry no real-person signal. Inferred-only overlap may be noted, not promoted.
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
| E2 | lead + methodology reviewer | high | raised by 2 → +1 |
| P1 | lead | normal | single source |
| FREE-01 | GIS reviewer (inferred) | normal | panel-specific, not in bank |

`FREE-XX` items are candidate new bank entries. In this public version they stay local to the run — there is no shared question bank to write back to.
