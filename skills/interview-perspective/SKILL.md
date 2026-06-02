---
name: interview-perspective
description: Distill a PI or committee perspective, or candidate digital twin, for PhD interview preparation. Uses the same academic distillation engine for both sides. Inputs: interviewer papers/projects or candidate capability network. Outputs: pi_perspective.md, committee_perspective.md, or candidate_twin.md.
---

# Interview Perspective Skill

Build a compact, operational perspective profile for PhD interview preparation.

This skill has two subject types:

| subject_type | Input | Output |
|---|---|---|
| `pi` | lead interviewer, optional panel members, papers, projects, job ad | `pi_perspective.md` or `committee_perspective.md` |
| `candidate` | candidate capability network, optional CV / SOP / CL | `candidate_twin.md` |

The same distillation engine is used for both. That symmetry is intentional.

---

## CONFIG

```text
DOC_HOME       = ~/.phd-interview-flow
METHODOLOGY    = $DOC_HOME/docs/perspective_methodology.md
COMMITTEE      = $DOC_HOME/docs/committee_perspectives.md
PI_TPL         = $DOC_HOME/templates/pi_perspective_template.md
COMMITTEE_TPL  = $DOC_HOME/templates/committee_perspective_template.md
TWIN_TPL       = $DOC_HOME/templates/candidate_twin_template.md
```

If `~/.phd-interview-flow/docs/` does not exist, prompt the user to follow QUICKSTART and copy repo `docs/` + `templates/` into `~/.phd-interview-flow/`.

---

## Inputs

### Required for PI

- `subject_type=pi`
- `lead_interviewer=<name + affiliation>` or `pi=<name + affiliation>`
- at least one of:
  - `papers_or_projects=<URL/text/path>`
  - `job_url=<URL>`
  - `job_text=<text>`
  - `intel.md=<path>`

Optional:

- `panel_members=<list of name + affiliation, or unknown committee member labels>`
- `interviewers=<structured list with role, name, affiliation, sources>`
- `unknown_panel_count=<number>`

### Required for candidate

- `subject_type=candidate`
- `capability_network=<path>`

Optional:

- `candidate_cv=<path/text>`
- `sop=<path/text>`
- `cover_letter=<path/text>`
- `output_dir=<path>`

---

## Workflow

### Step 1: Load Sources

Read provided files or text. If URLs are provided and web access is available, use primary sources first:

- official lab or university page
- PI profile
- papers / abstracts
- grants / project pages
- job ad

If a URL cannot be read, ask the user to paste the relevant text. Mark missing details `(to verify)`.

### Step 2: Extract Dimensions

Use `$METHODOLOGY`.

For PI:

- core research questions
- methodological preferences
- evidence standards
- recurring tradeoffs
- likely interview concerns
- what answers may sound thin

For multiple interviewers:

- identify one lead interviewer
- build one perspective per known interviewer
- for unknown committee members, infer two likely disciplinary perspectives from `$COMMITTEE`
- mark inferred perspectives exactly as `(inferred, no source)`
- keep lead perspective dominant and panel perspectives as probe lenses

For candidate:

- credible evidence inventory
- repeated problem types
- methods actually used
- working-style patterns
- strong answer zones
- thin answer zones

### Step 3: Synthesize Operational Profile

Do not write a biography. Write a profile that can generate questions and evaluate answers.

For PI or committee, answer:

```text
What would the lead interviewer ask?
What would each panel member probe from their own lens?
What would they consider generic?
What would count as source-backed fit?
```

For candidate, answer:

```text
What can this candidate credibly say?
Where is evidence strong?
Where would the answer become generic?
What requires user confirmation?
```

### Step 4: Write Output

Default output:

```text
<output_dir>/pi_perspective.md
<output_dir>/committee_perspective.md
<output_dir>/candidate_twin.md
```

If `output_dir` is absent:

```text
./interview_run/
```

Use `$PI_TPL`, `$COMMITTEE_TPL`, or `$TWIN_TPL`.

Write `committee_perspective.md` when there is more than one interviewer or any unknown committee member. Otherwise write `pi_perspective.md`.

### Step 5: Quality Check

Before finishing, verify:

- every major claim has a source note
- no private path or personal example leaked
- no invented achievements, numbers, or publications
- honest boundary is present
- output can generate interview questions

---

## Candidate Twin Integrity Rule

The candidate twin must stay inside the capability network. If the network lacks evidence, say so.

Bad:

```text
The candidate can discuss advanced fieldwork leadership.
```

Good:

```text
The provided capability network does not include fieldwork leadership evidence. In twin mode, answers on this topic should be marked thin unless the user supplies a concrete example.
```

---

## Related

- [`interview-mock`](../interview-mock/SKILL.md) uses this skill to build PI perspective and candidate twin.
- Methodology: `$METHODOLOGY`
