# Workflow Methodology

`phd-interview-flow` has one shared core and two answer modes.

```text
Phase 0  input completion and confirmation
Phase 1  PI / committee perspective distillation
Phase 1b candidate twin distillation, twin mode only
Phase 2  interviewer question generation and bank comparison
Phase 3  prep page generation
Phase 4  answer collection
Phase 5  D1-D5 evaluation and Gap Report
```

---

## Shared Core

The shared core is:

```text
distill committee perspective -> use interviewer perspectives to generate questions -> evaluate answers against D1-D5
```

The answer source is the only branch point.

| Mode | Answer source | Main loop |
|---|---|---|
| `companion` | User answers directly | Ask one question, wait, evaluate, repeat |
| `twin` | Candidate digital twin answers automatically | Generate full session, evaluate all answers |

---

## Phase 0: Input Completion

See [input_phase0.md](input_phase0.md).

Phase 0 accepts multiple entry points:

- PI name and affiliation
- PI papers, lab page, project page, or job ad
- candidate CV
- candidate capability network
- existing intel.md

The rule is: use what exists, deepen it, and build the missing sections. After writing or updating `intel.md`, stop and wait for `confirm`.

---

## Phase 1: PI / Committee Perspective

Run `interview-perspective` with:

- `subject_type=pi`
- lead interviewer name and affiliation
- optional panel members
- papers / projects / job ad / interview context

Output:

- one interviewer: `pi_perspective.md`
- multiple interviewers or unknown panel members: `committee_perspective.md`

The goal is not biography. The useful output is a compact operational profile:

- what the lead interviewer likely cares about
- what methods each known interviewer trusts
- what tradeoffs each panel member may probe
- what answer types will sound thin
- what questions they are likely to ask

For unknown committee members, infer 2 likely disciplinary perspectives from project methods and field context. Mark each inferred perspective `(inferred, no source)`.

See [committee_perspectives.md](committee_perspectives.md).

---

## Phase 1b: Candidate Twin

Only for `mode=twin`.

Run the same `interview-perspective` distillation engine with:

- `subject_type=candidate`
- candidate capability network
- optional CV / SOP / CL / project notes

Output: `candidate_twin.md`.

The twin is not a personality clone. It is a preparation proxy: what this candidate can credibly answer based on their documented capability network.

If no capability network is available, do not invent a twin. Ask the user to build one with Step 01 or switch to `mode=companion`.

---

## Phase 2: Question Generation

Use `pi_perspective.md` or `committee_perspective.md` to generate interview questions.

Question categories:

- E: Experience
- W: Working Style
- P: Passion
- S: Synthesis

Use [templates/question_bank_template.md](../templates/question_bank_template.md) as the shape. The PI perspective may create new `FREE-XX` questions when the generic bank does not cover a likely line of questioning.

For a committee:

- lead interviewer generates the main question set
- panel members add probes from their own lens
- inferred perspectives add only clearly marked inferred probes

Compare each generated question to the generic bank:

- hit: map to the closest `question_id`
- variant: map to the closest `question_id` and mark `variant`
- novel: assign `FREE-XX`

If the same question or mapped `question_id` is raised by 2 or more interviewers, increase priority by 1. Higher priority questions appear first in the prep page and the practice loop.

See [question_weighting.md](question_weighting.md).

---

## Phase 3: Prep Page

Assemble:

- PI perspective summary
- likely questions
- question priority and `asked_by`
- candidate evidence map
- answer outlines
- questions the candidate can ask the PI
- final checklist

Output target is auto-detected:

- Notion available: offer Notion structured page
- Notion unavailable: write `interview_prep.md`

See [output_targets.md](output_targets.md).

---

## Phase 4: Answer Collection

### companion

Ask one PI-perspective question at a time:

1. Present question.
2. Wait for user's answer.
3. Evaluate with D1-D5.
4. Give a short repair suggestion.
5. Continue to next question or repeat a revised answer.

Do not auto-answer for the user in this mode.

### twin

The candidate twin answers the full session automatically. The answer must stay inside the capability network. If evidence is missing, the twin should answer thinly or mark the gap, not invent experience.

In committee runs, the lead interviewer drives the session and panel members insert probes. Twin transcripts must label which interviewer asks each question.

---

## Phase 5: Evaluation

Use [d1_d5_rubric.md](d1_d5_rubric.md).

Output:

- per-question score
- concrete gap
- missing evidence source
- repair action
- overall Gap Report

The Gap Report judges preparation coverage. It should not make claims about the candidate's worth.
