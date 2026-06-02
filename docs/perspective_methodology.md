# Perspective Distillation Methodology

`interview-perspective` is a simplified public adaptation of an academic person-thinking distillation workflow.

It has two uses:

1. PI -> PI perspective
2. Candidate -> digital twin

The same engine is reused for both. This symmetry is central to the repo.

---

## PI Perspective

Input:

- PI name and affiliation
- lab / profile / project page
- selected papers or abstracts
- job ad or PhD project text
- optional interview format

Output:

- research priorities
- methodological preferences
- evidence standards
- likely concerns about candidates
- questioning habits
- likely questions
- honest boundary

Good PI perspective output should help answer:

```text
What would this PI notice?
What would they distrust?
What would make them ask a follow-up?
What answer would feel unusually prepared?
```

---

## Candidate Twin

Input:

- capability network from Step 01
- optional CV / SOP / cover letter / project notes

Output:

- credible evidence inventory
- recurring working patterns
- research taste and constraints
- answerable question types
- likely thin areas
- honest boundary

The twin must not invent capability. It can only use evidence present in the source network or explicitly provided materials.

---

## Extraction Dimensions

Use these dimensions for both PI and candidate.

| Dimension | PI version | Candidate version |
|---|---|---|
| Core questions | What problems does the PI keep returning to? | What problems has the candidate repeatedly worked on? |
| Method taste | What methods / evidence does the PI trust? | What methods / evidence can the candidate credibly discuss? |
| Tradeoffs | What tensions appear in the PI's work? | What tradeoffs has the candidate actually navigated? |
| Evaluation habits | What makes an answer persuasive to this PI? | What makes this candidate's answer specific? |
| Blind spots | What public data is missing? | What preparation evidence is missing? |

---

## Honest Boundary

Every perspective file must include:

- source date
- sources used
- sources missing
- what should not be inferred

Example:

```text
This profile is based on public project pages and three paper abstracts. It should not infer private lab culture, funding decisions, or the PI's personal interview behavior.
```

---

## Quality Checks

A useful perspective is:

- operational: it can generate questions
- falsifiable: it names source-backed reasons
- bounded: it admits missing data
- non-generic: it avoids "interdisciplinary, rigorous, motivated" unless evidence explains why

A weak perspective is:

- a biography
- a publication list
- a generic "good PI" profile
- a personality guess
- a copy of the job ad
