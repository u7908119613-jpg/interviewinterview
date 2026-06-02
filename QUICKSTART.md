# Quickstart · AI Agent Setup Guide

> Audience: AI coding agent (Claude Code / Cursor / Cline / Codex)
> Goal: 安装 `interview-perspective` + `interview-mock` 两个 skill，跑通一次「intel.md / PI URL -> prep page -> companion or twin evaluation」。

---

## Task 1 · 定位仓库

```bash
REPO_PATH=/absolute/path/to/phd-interview-flow
test -f "$REPO_PATH/skills/interview-perspective/SKILL.md"
test -f "$REPO_PATH/skills/interview-mock/SKILL.md"
test -f "$REPO_PATH/templates/intel_template.md"
test -f "$REPO_PATH/templates/question_bank_template.md"
```

---

## Task 2 · 准备本地资源

skill 通过 `~/.phd-interview-flow/` 路径引用方法论 docs 和模板。安装时把整套 docs + templates 拷过去：

```bash
mkdir -p "$HOME/.phd-interview-flow"
cp -r "$REPO_PATH"/docs "$HOME/.phd-interview-flow/"
cp -r "$REPO_PATH"/templates "$HOME/.phd-interview-flow/"
ls "$HOME/.phd-interview-flow/"
```

应看到 `docs/` 和 `templates/` 两个目录，分别含：

- `docs/`: `workflow.md`、`input_phase0.md`、`perspective_methodology.md`、`d1_d5_rubric.md`、`output_targets.md`
- `templates/`: `intel_template.md`、`question_bank_template.md`、`pi_perspective_template.md`、`candidate_twin_template.md`、`prep_page_template.md`、`gap_report_template.md`

---

## Task 3 · 装 skill

### Claude Code

```bash
mkdir -p "$HOME/.claude/skills"
cp -r "$REPO_PATH"/skills/interview-perspective "$HOME/.claude/skills/"
cp -r "$REPO_PATH"/skills/interview-mock "$HOME/.claude/skills/"
```

首次运行：

```text
/interview-mock mode=companion pi="Fictional PI, Example University" job_url=<URL>
/interview-mock mode=twin intel.md=<path> capability_network=<path>
```

多面试官 / committee：

```text
/interview-mock mode=companion lead_interviewer="Fictional Lead, Example University" panel_members="Fictional Panelist (methods); unknown committee member" job_url=<URL>
```

### Cursor

```bash
mkdir -p "$REPO_PATH/.cursor/rules"
cp "$REPO_PATH"/skills/interview-perspective/SKILL.md "$REPO_PATH/.cursor/rules/interview-perspective.md"
cp "$REPO_PATH"/skills/interview-mock/SKILL.md "$REPO_PATH/.cursor/rules/interview-mock.md"
```

首次运行：

```text
Read .cursor/rules/interview-mock.md and run it.
Parameters:
  mode = companion
  pi = <PI name + affiliation>
  papers_or_projects = <URL or pasted text>
```

### Cline

```bash
cat "$REPO_PATH"/skills/interview-perspective/SKILL.md >> "$REPO_PATH/.clinerules"
cat "$REPO_PATH"/skills/interview-mock/SKILL.md >> "$REPO_PATH/.clinerules"
```

首次运行：

```text
Run interview-mock in companion mode on PI/project materials at <path or URL>.
```

### Codex

Codex 可直接读取 skill 文件运行：

```text
Read /absolute/path/to/phd-interview-flow/skills/interview-mock/SKILL.md and run it.
Parameters:
  mode = companion
  pi = <PI name + affiliation>
  job_url = <URL or pasted job text>
  candidate_cv = <path or pasted text>
```

---

## Task 4 · 准备输入

### 最小输入

任意一种都可以启动：

| 输入 | 参数 |
|---|---|
| 完整 intel.md | `intel.md=<path>` |
| PI + 项目材料 | `pi=<name + affiliation>` + `papers_or_projects=<URL/text>` |
| 招聘公告 | `job_url=<URL>` 或 `job_text=<pasted text>` |
| 已有能力网络 | `capability_network=<path>` |

Phase 0 会补齐缺失段，写出 `intel.md`，然后停下等用户 `confirm`。

### twin 模式额外输入

`mode=twin` 需要候选人的能力网络作为分身来源：

```text
capability_network=<path to Step 01 capability network>
```

若没有能力网络，agent 应提示用户先运行：

```text
https://github.com/Noah0025/obsidian-capability-network
```

用户也可以降级为：

```text
mode=companion
```

---

## Task 5 · 首次运行

### 5.1 陪练模式 companion

```text
Run interview-mock.
Parameters:
  mode = companion
  pi = <PI name + affiliation>
  papers_or_projects = <URL or pasted text>
  candidate_cv = <path or pasted text>
  output_dir = ./interview_run
```

期望行为：

1. Phase 0 补齐 intel.md，列出待核验字段，STOP 等 `confirm`
2. Phase 1 用 `interview-perspective` 蒸馏 PI perspective
3. Phase 2 让 PI perspective 生成问题
4. Phase 3 生成 prep page，Notion 可用则询问是否写 Notion，否则写 markdown
5. Phase 4 逐题提问，等待用户真实回答
6. Phase 5 每题用 D1-D5 给反馈，并把薄弱项写入 Gap Report

### 5.2 分身模式 twin

```text
Run interview-mock.
Parameters:
  mode = twin
  intel.md = <path>
  capability_network = <path>
  output_dir = ./interview_run
```

期望行为：

1. Phase 0 校验 intel.md + capability network，STOP 等 `confirm`
2. Phase 1 蒸馏 PI perspective
3. Phase 1b 用同一个 distillation engine 蒸馏 candidate twin
4. Phase 2 让 PI perspective 生成问题
5. Phase 3 生成 prep page
6. Phase 4 由 candidate twin 自动回答完整 session
7. Phase 5 输出 Gap Report，重点标出「准备薄」而不是「人不行」

---

## Task 6 · Notion 自动检测

Notion 是可选输出，不是依赖。agent 应在运行时检测是否有 Notion MCP / connector：

```text
1. 列出可用 MCP resources / connectors / tools
2. 如果发现 Notion 能力：询问用户是否写入 Notion
3. 如果没有：写 markdown 文件，不报错
```

输出文件名：

```text
<output_dir>/intel.md
<output_dir>/pi_perspective.md
<output_dir>/candidate_twin.md       # twin mode only
<output_dir>/interview_prep.md
<output_dir>/gap_report.md
```

详见 `docs/output_targets.md`。

---

## 验收检查

- [ ] Phase 0 后停下等 `confirm`
- [ ] `mode=companion` 逐题等待用户真实回答
- [ ] `mode=twin` 只在有 capability network 时自动回答
- [ ] PI perspective 和 candidate twin 使用同一套 distillation engine
- [ ] D1-D5 分数来自回答内容，不凭印象
- [ ] Notion 不可用时能正常落 markdown
- [ ] 所有输出不含私人路径、真实姓名示例、私有题库内容
