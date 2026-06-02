# D1-D5 Rubric

D scores measure answer differentiation in a PhD interview.

They do not measure personal worth. They measure whether the answer gives the PI a reason to remember the candidate.

---

## Scale

| Score | Label | Meaning |
|---|---|---|
| D5 | Earned Secret | The answer contains a concrete, non-obvious insight that only this candidate is likely to have earned through their work. |
| D4 | Specific Beyond Generic | The answer has concrete numbers, tradeoffs, scenes, decisions, or failure modes that go beyond a standard prepared answer. |
| D3 | Correct but Replaceable | The answer is coherent and complete, but another prepared candidate could say the same thing. |
| D2 | Mostly Cliche | The answer relies on common phrases, values, or enthusiasm without enough evidence. |
| D1 | No Differentiation | The answer is generic, evasive, or disconnected from the question. |

---

## Evidence Markers

| Marker | Usually raises score |
|---|---|
| Specific research decision | D3 -> D4 |
| Concrete failure and repair | D3 -> D4 |
| Number / dataset / timeline / tool with relevance | D3 -> D4 |
| PI-specific bridge | D3 -> D4 |
| Non-obvious lesson earned from prior work | D4 -> D5 |

| Marker | Usually lowers score |
|---|---|
| "I am passionate about..." without evidence | D2 |
| "I am a fast learner..." without scene | D2 |
| Listing methods without why they mattered | D2-D3 |
| Repeating the job ad | D2-D3 |
| Invented or unverifiable claim | Do not score; flag integrity risk |

---

## Per-Question Feedback Format

```markdown
### Feedback

- D score: D3
- Why: The answer is structurally correct but replaceable.
- Missing: one concrete decision, constraint, or result from the candidate's own work.
- Repair: add a 20-second example with situation -> decision -> result -> bridge to this PI.
```

---

## Gap Report Format

Use [../templates/gap_report_template.md](../templates/gap_report_template.md).

Required columns:

| Column | Meaning |
|---|---|
| `question_id` | E/W/P/S ID or `FREE-XX` |
| `topic` | short topic label |
| `asked_by` | lead / panel / inferred |
| `status` | green / yellow / red |
| `D` | D1-D5 |
| `gap` | concrete weakness |
| `repair` | next edit or practice action |
| `source_needed` | capability network / CV / intel / user confirmation |

---

## FREE Questions

If the PI perspective asks something outside the prepared question bank:

```text
FREE-01, FREE-02, ...
```

FREE questions are useful signals. They either reveal:

- a PI-specific concern
- a missing category in the prep page
- a candidate evidence gap

Do not automatically add every FREE question to the question bank. Mark it in the Gap Report first.
