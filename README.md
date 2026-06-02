# phd-interview-flow

> PhD 申请面试准备 workflow：从 PI 视角蒸馏出发，生成面试问题，支持陪练模式和分身模式，并用 D1-D5 标尺复盘准备漏洞。

---

> **本 README**：写给人类，解释这是什么 / 放在 workflow 哪一步 / 怎么接入。
> **[QUICKSTART.md](QUICKSTART.md)**：写给 AI agent，按工具安装并跑通一次面试准备。

---

## 这是什么

`phd-interview-flow` 是 PhD 申请 5 步 workflow 的 Step 05。

```text
Step 01 能力网络      -> Noah0025/obsidian-capability-network
Step 02 搜索 + 初评    -> Noah0025/phd-scout-flow
Step 03 深度调研       -> Noah0025/phd-intel-brief
Step 04 文书定制       -> Noah0025/phd-doc-flow
Step 05 面试准备       -> Noah0025/phd-interview-flow  [当前]
```

各步骤独立可用，整套串联效果最好。

它做三件事：

1. **PI 视角**：根据 PI 姓名、机构、论文、项目和招聘信息，蒸馏出 PI 可能关心什么、偏好什么方法、会怎样追问。
2. **问题生成**：让 PI 视角反向生成面试问题，按 Experience / Working Style / Passion / Synthesis 分类。
3. **D1-D5 复盘**：根据真实回答或数字分身回答，判断答案是 generic 还是有独特准备，并输出 Gap Report。

---

## 两种模式

两种模式共用同一个核心链路，只区别于**谁来回答**。

```text
PI 材料 -> distill PI perspective -> PI perspective 生成问题 -> [谁回答] -> D1-D5 评估
```

| 模式 | 谁回答 | 适合什么时候 | 输出 |
|---|---|---|---|
| `companion` 陪练 | 用户真实回答 | 低门槛、临面前练习、想逐题修正 | 逐题反馈 + prep page |
| `twin` 分身 | 候选人的 digital twin 自动回答 | 已有能力网络，想发现准备盲区 | 完整 mock transcript + Gap Report |

**对称性**：同一个蒸馏引擎既用于 PI，也用于候选人。

- PI -> PI perspective：从公开论文 / 项目 / 招聘信息中推断面试官关注点。
- Candidate -> twin：从候选人的能力网络中推断可被调用的经历、表达习惯和证据素材。

`twin` 模式的输入源应优先来自 Step 01 的能力网络：<https://github.com/Noah0025/obsidian-capability-network>。没有能力网络时，workflow 会提示先构建；也可以降级到 `companion` 模式。

---

## 两个 skill

| Skill | 触发 | 输出 |
|---|---|---|
| `interview-perspective` | 蒸馏 PI / 蒸馏候选人分身 / build perspective | `pi_perspective.md` 或 `candidate_twin.md` |
| `interview-mock` | 面试陪练 / mock interview / twin mode | `interview_prep.md` + `gap_report.md` 或 Notion 页面 |

---

## 委员会 / 多面试官

PhD 面试常是 committee，两个 skill 都支持多位面试官：

- **已知面试官**：分 lead（主导面试节奏）+ panel（从各自专业视角补追问），各自蒸馏视角。
- **未知委员**：不臆造身份，按项目领域推断 2 个最可能的学科视角，标注 `(inferred, no source)`。
- **题目加权**：同一问题被 ≥2 位面试官问到 → 优先级 +1，准备页和练习按优先级排序——委员会共同关心的先练。

输入：`lead_interviewer=` + `panel_members=` / `interviewers=`（结构化列表）/ `unknown_panel_count=`。

详见 [docs/committee_perspectives.md](docs/committee_perspectives.md) 与 [docs/question_weighting.md](docs/question_weighting.md)。

---

## 三种入口（Phase 0 自动判断）

`phd-interview-flow` 自带 Phase 0。原则是：**有就继续深入，没有的 AI 自建**。

| 入口 | 用户给的 | Phase 0 做什么 |
|---|---|---|
| A · 完整 intel.md | PI / 项目 / 岗位 / 候选人素材已整理 | 校验完整度，进入 Phase 1 |
| B · 部分材料 | PI 名字、机构、论文、项目 URL、招聘文本、CV、能力网络中的任意几项 | 补齐缺失段，标注待核验字段 |
| C · 只有 PI 名或 URL | 一个 PI 名称、实验室页、项目页或招聘公告 | 自主调研可公开信息，生成第一版 intel.md |

Phase 0 完成后必须停下，等用户回复 `confirm` 才进入面试准备。原因很简单：PI 视角和候选人素材一旦错，后续问题和复盘都会跟着错。

详见 [docs/input_phase0.md](docs/input_phase0.md)。

---

## 输出位置：Notion 可选，不依赖

这个 repo **不依赖 Notion**。

运行时 agent 应检测当前环境是否有 Notion MCP / Notion connector：

- **有**：询问用户是否把 prep page 和 Gap Report 写入 Notion 结构化页面。
- **没有**：自动回退为当前工作目录下的 markdown 文件。

默认 markdown 输出：

```text
interview_prep.md
gap_report.md
pi_perspective.md
candidate_twin.md
```

详见 [docs/output_targets.md](docs/output_targets.md)。

---

## Generic Question Bank

本 repo 不内置私有题库内容，只提供通用题库模板。

| 类别 | 含义 |
|---|---|
| E · Experience | 过去研究、项目、失败、方法选择 |
| W · Working Style | 和导师协作、遇到卡点、独立推进、反馈循环 |
| P · Passion | 为什么这个方向、为什么这个 PI、长期问题意识 |
| S · Synthesis | 把经历、PI 项目和未来研究问题串起来 |

模板见 [templates/question_bank_template.md](templates/question_bank_template.md)。

---

## D1-D5 标尺

| 分数 | 含义 |
|---|---|
| D5 | 有 earned secret：说出反直觉、具体、属于自己的洞察 |
| D4 | 具体细节明显超出 generic 答案 |
| D3 | 正确完整，但换个候选人也能说 |
| D2 | 套话为主，缺证据 |
| D1 | 零区分度 |

完整规则见 [docs/d1_d5_rubric.md](docs/d1_d5_rubric.md)。

---

## 保守承诺

这个 workflow 沉淀的是「把 PI 情报和候选人素材转化成面试准备」的可复用流程。

它不替代：

- 你对研究方向的真实判断
- 你和 PI 的实际交流
- 面试前的人工校验

也不保证：

- 一次生成完美答案
- 数字分身能代表真实发挥
- 公开资料能完整反映 PI 的面试风格

---

## 安装

把 [QUICKSTART.md](./QUICKSTART.md) 整份交给你的 AI agent。

人类手动安装也可以按 QUICKSTART 走，约 5-10 分钟。

---

## License

MIT
