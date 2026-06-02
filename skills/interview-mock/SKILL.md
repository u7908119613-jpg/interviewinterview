---
name: interview-mock
description: Run PhD interview preparation in companion or twin mode. Shared core: committee perspective -> interviewer-generated questions -> answer source -> D1-D5 evaluation. Notion output is optional and auto-detected; markdown is the fallback.
---

# Interview Mock Skill

Prepare for a PhD interview with one shared engine and two answer modes.

```text
interviewer materials -> committee perspective -> interviewer-generated questions -> [answer source] -> D1-D5 evaluation
```

| mode | Answer source |
|---|---|
| `companion` | user answers live, one question at a time |
| `twin` | candidate digital twin answers automatically from capability network |

---

## CONFIG

```text
DOC_HOME       = ~/.phd-interview-flow
WORKFLOW       = $DOC_HOME/docs/workflow.md
PHASE0         = $DOC_HOME/docs/input_phase0.md
RUBRIC         = $DOC_HOME/docs/d1_d5_rubric.md
OUTPUTS        = $DOC_HOME/docs/output_targets.md
COMMITTEE      = $DOC_HOME/docs/committee_perspectives.md
WEIGHTING      = $DOC_HOME/docs/question_weighting.md
INTEL_TPL      = $DOC_HOME/templates/intel_template.md
QBANK_TPL      = $DOC_HOME/templates/question_bank_template.md
PREP_TPL       = $DOC_HOME/templates/prep_page_template.md
GAP_TPL        = $DOC_HOME/templates/gap_report_template.md
```

If `~/.phd-interview-flow/docs/` does not exist, prompt the user to follow QUICKSTART and copy repo `docs/` + `templates/` into `~/.phd-interview-flow/`.

---

## Inputs

### Required

- `mode=companion` or `mode=twin`
- at least one PI/project source:
  - `intel.md=<path>`
  - `pi=<name + affiliation>`
  - `lead_interviewer=<name + affiliation>`
  - `interviewers=<structured list with role, name, affiliation, sources>`
  - `papers_or_projects=<URL/text/path>`
  - `job_url=<URL>`
  - `job_text=<text>`

### Required for twin

- `capability_network=<path>`

If `mode=twin` and no capability network exists, stop and ask the user to either:

1. build a capability network with <https://github.com/Noah0025/obsidian-capability-network>, or
2. rerun with `mode=companion`.

### Optional

- `candidate_cv=<path/text>`
- `panel_members=<list of name + affiliation, or unknown committee member labels>`
- `unknown_panel_count=<number>`
- `output_dir=<path>`
- `confirm=true` for non-interactive continuation after Phase 0
- `rounds=<number>` for twin mode, default `1`

---

## Workflow

### Phase 0: Input Completion

Follow `$PHASE0`.

Create or update `intel.md` from whatever the user supplied:

- existing intel
- PI name and affiliation
- lead interviewer and panel members, if known
- papers / projects / URLs / pasted text
- job ad
- candidate CV
- capability network

After writing `intel.md`, stop:

```text
intel.md 已写入 <path>。请 review，特别确认：
- PI / project / interview logistics
- 3-5 条 likely evaluation priorities
- candidate evidence map
- 所有 (to verify) 字段

回复 `confirm` 进入 Phase 1；
或列出需要修改的项。
```

Do not continue without `confirm` unless `confirm=true` is explicitly provided.

### Phase 1: Committee Perspective

Run `interview-perspective`:

```text
subject_type=pi
intel.md=<path>
lead_interviewer=<optional>
interviewers=<optional>
panel_members=<optional>
unknown_panel_count=<optional>
output_dir=<output_dir>
```

Output: `pi_perspective.md` for one interviewer, or `committee_perspective.md` for multiple interviewers / unknown panel members.

### Phase 1b: Candidate Twin

Only for `mode=twin`.

Run `interview-perspective`:

```text
subject_type=candidate
capability_network=<path>
candidate_cv=<optional>
output_dir=<output_dir>
```

Output: `candidate_twin.md`.

### Phase 2: Interviewer-Generated Questions

Use `pi_perspective.md` or `committee_perspective.md` and `$QBANK_TPL` to generate questions per interviewer.

Suggested volume:

- lead interviewer: 10 questions across E / W / P / S
- known panel member: 5 questions from that member's lens
- inferred unknown perspective: 3-5 questions, marked `(inferred, no source)`
- optional `FREE-XX` questions for concerns outside the generic bank

Each question should include:

- `question_id`
- exact question
- `asked_by`
- `role`
- match result: hit / variant / novel
- why this interviewer may ask it
- what evidence would make it D4/D5
- likely follow-up

Compare each generated question against `$QBANK_TPL`:

- semantic hit -> map to the closest E/W/P/S `question_id`
- near match -> map to the closest `question_id` and mark `variant`
- novel -> assign `FREE-XX`

Apply `$WEIGHTING`: if the same question or mapped `question_id` appears from 2 or more interviewers, increase priority by 1. Higher-priority questions are ordered first in Phase 3 and in the Phase 4 practice loop.

### Phase 3: Prep Page

Use `$PREP_TPL`.

Include:

- PI quick profile
- interview priorities
- selected questions sorted by priority, then lead-interviewer questions, then panel probes
- candidate evidence map
- questions to ask the PI
- final checklist

Then detect output target using `$OUTPUTS`:

- Notion available: offer structured Notion page
- Notion absent: write markdown

Never require Notion.

### Phase 4A: companion Mode

Loop over questions one at a time, using the Phase 3 priority order.

For each question:

1. Ask the question and show `asked_by` / role.
2. Wait for the user's real answer.
3. Score with D1-D5 using `$RUBRIC`.
4. Give concise feedback:
   - score
   - why
   - missing evidence
   - one repair action
5. Ask whether to retry, continue, or stop.

Do not generate the user's answer in companion mode.

### Phase 4B: twin Mode

Generate a full mock session.

Rules:

- lead interviewer drives the session.
- panel members insert probes from their own perspectives.
- inferred unknown perspectives may ask probes only when marked `(inferred, no source)`.
- label each interviewer turn with `asked_by`.
- Candidate twin answers only from `candidate_twin.md`.
- If evidence is missing, the twin gives a thin answer or marks the gap.
- Do not invent achievements, numbers, or motives.
- Include follow-ups when an answer is D1-D3 or when the PI perspective suggests a likely probe.

### Phase 5: Evaluation And Gap Report

Use `$RUBRIC` and `$GAP_TPL`.

For each answer:

- assign D1-D5
- mark green / yellow / red
- describe the gap concretely
- name the source needed to repair it
- add FREE questions if the PI asked outside the bank

Write:

```text
<output_dir>/gap_report.md
```

If Notion was selected and available, also append or update the Notion page.

---

## Output Files

Default:

```text
<output_dir>/intel.md
<output_dir>/pi_perspective.md
<output_dir>/interview_prep.md
<output_dir>/gap_report.md
```

Twin mode also writes:

```text
<output_dir>/candidate_twin.md
```

If `output_dir` is absent:

```text
./interview_run/
```

---

## Error Handling

| Problem | Behavior |
|---|---|
| No PI/project input | Ask for PI name, project URL, job ad, or pasted text |
| URL cannot be read | Ask user to paste text |
| Phase 0 has unverified fields | Mark `(to verify)` and stop for confirm |
| twin mode without capability network | Stop; suggest Step 01 or companion mode |
| Notion unavailable | Markdown fallback |
| Notion fails | Markdown fallback and report failure |
| Answer contains invented claim | Flag integrity risk; do not score as D4/D5 |

---

## Related

- [`interview-perspective`](../interview-perspective/SKILL.md)
- `$WORKFLOW`
- `$RUBRIC`
- `$OUTPUTS`
