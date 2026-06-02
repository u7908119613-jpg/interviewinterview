# Phase 0 Input Handling

Phase 0 makes the workflow runnable from messy inputs.

Core rule:

> 有就继续深入，没有的 AI 自建。

After Phase 0 writes or updates `intel.md`, stop and wait for the user's exact `confirm` before Phase 1.

---

## Accepted Inputs

| Input | Examples | Required? |
|---|---|---|
| PI | name, affiliation, lab page | At least one PI or project clue is needed |
| Papers / projects | URLs, pasted abstracts, project pages | Optional but strongly useful |
| Job ad | URL or pasted text | Optional |
| Candidate CV | path or pasted text | Optional in companion, useful in twin |
| Capability network | Step 01 export / folder / markdown files | Required for twin |
| Existing intel.md | prior research file | Optional |

---

## Output: intel.md

Phase 0 should create or update `intel.md` using [../templates/intel_template.md](../templates/intel_template.md).

Suggested default path:

```text
<output_dir>/intel.md
```

If `output_dir` is not provided:

```text
./interview_run/intel.md
```

If the target path already exists, do not silently overwrite. Ask the user:

```text
skip / overwrite / merge
```

---

## Completion Rules

`intel.md` is complete enough for Phase 1 when it has:

- PI or interviewer identity, even if some details are marked `(to verify)`
- project / role / lab context
- 3-5 likely evaluation priorities
- candidate evidence map, if CV or capability network exists
- interview logistics if known
- source notes with URLs or pasted-source labels

Unknown fields are allowed, but must be explicit:

```text
(to verify)
```

Never invent:

- deadlines
- funding status
- PI names
- publications
- candidate achievements
- interview format

---

## Research Fallbacks

When a URL is unavailable, login-gated, PDF-only, or JavaScript-only:

1. Tell the user what could not be read.
2. Ask the user to paste the relevant text.
3. Continue with pasted text as source.
4. Mark missing details `(to verify)`.

If autonomous web research is available, prefer primary sources:

- official lab / university pages
- PI profile
- publications
- grant pages
- project pages
- job ad

Use secondary summaries only as weak context.

---

## Mandatory STOP

After Phase 0:

```text
intel.md 已写入 <path>。请 review，特别确认：
- PI / project / interview logistics
- 3-5 条 likely evaluation priorities
- candidate evidence map
- 所有 (to verify) 字段

回复 `confirm` 进入 Phase 1；
或列出需要修改的项。
```

Do not continue to Phase 1 until the user confirms or revises.

For batch mode, require `confirm=true` on the next invocation. The default is interactive safety.
