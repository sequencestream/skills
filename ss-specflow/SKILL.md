---
name: ss-specflow
description: A lightweight spec-driven development flow that takes one requirement through a four-step trunk — requirement → spec → plan → develop — run inline, where develop covers coding, testing, and project-doc sync as one complete delivery. On top of the trunk sits a single enhancement-judgement step that decides whether to enable optional review sub-agents (spec-review, plan-review, develop-review); each review feeds its opinions back to the matching trunk step, which decides on its own whether to improve before continuing. Trigger when the user supplies a requirement (or a session directory to resume) and wants a structured but low-ceremony flow — no complexity matrix, no fix-loop counters. A leaner parallel to ss-develop-pipeline.
argument-hint: [requirement description or session directory]
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Agent
---

Run a lightweight spec-driven flow. The **trunk** runs inline; a single **enhancement-judgement** step decides whether to add review sub-agents. Keep it simple — no complexity matrix, no fix-loop counters.

## Instructions

1. Resolve `<root-doc-dir>` (user-specified, else check `specs/`, then `doc/`) and `<root-session-dir>` (user-specified, else `changes/`).
2. Resolve `<session-dir>`:
   - If the user specifies one, use it.
   - Else if the user supplies a requirement, create `<root-session-dir>/<YYYY>/<MM>/<DD>/YYYY-MM-DD-<NNN>-<requirement-short-name>/` (`<NNN>` = zero-padded daily sequence from 001) and write the raw requirement to `<session-dir>/requirement.md`.
   - Else ask the user for one of the two.
3. Run the trunk in order: requirement → spec → plan → develop. After the `spec` and `plan` steps, hit the **enhancement judgement** to decide whether to dispatch the matching review sub-agent; after `develop`, optionally dispatch `develop-review`. Resume by reading existing artifacts and continuing from the first missing one.
4. On completion, output a summary noting which reviews ran and what changed.

## 文件关系

| 文件 | 由哪一步产出 | 承载什么 |
| --- | --- | --- |
| `requirement.md` | requirement | 用户的**原始需求**原文(及澄清问答),不加工。 |
| `spec.md` | spec | **结构化需求**:背景、意图、功能场景与验收标准、约束、范围。描述 what,不写 how。 |
| `plan.md` | plan | **技术方案**:实现思路、涉及文件/模块、关键取舍、测试与文档同步要点。描述 how。 |

实现代码、测试、项目级文档(如 project-level spec)由 develop 步骤直接写入项目对应位置,不在 `<session-dir>` 单独留产物。

## 流转关系

主干四步内联运行;增强步骤全部以 subagent 运行,各自把意见回馈给对应主干步骤,由该步骤自行决定是否采纳改进,然后继续往下走。

```
requirement -> spec --(judge?)--> plan --(judge?)--> develop --(judge?)--> done
                 ^                  ^                    ^
                 |                  |                    |
            [spec-review†]     [plan-review†]     [develop-review†]
            (feedback back)    (feedback back)    (feedback back)
```

`†` = 以 subagent 运行;`[brackets]` = 仅当增强判断决定需要时才启用。

**增强判断环节**:每个主干步骤产出后,判断该步骤是否值得一次独立 review(涉及核心/共享逻辑、数据模型或接口、非平凡取舍、多文件改动、或存在不确定点时倾向启用;琐碎、机械、单点改动时跳过)。判断从简,不用评估矩阵;拿不准就启用。

**反馈机制**:review subagent 把意见写清后返回;对应主干步骤读取意见,**自行决定**采纳哪些、是否修订对应文件(spec.md / plan.md)或代码,然后继续下一步。不强制全盘接受,不计修复回环次数。

### Review subagent 派发

通过 `Agent` 工具派发,`subagent_type` 用 `general-purpose`,prompt 须自包含:角色定位、要 review 的文件的实际路径(替换占位符)、该步骤的核心原则、以及「返回一份可被对应步骤消费的意见(格式自定),不直接改主干文件」。返回后由主干步骤决定如何应用。

## 每个步骤的核心原则

- **requirement**:忠实记录原始需求;对不清楚处提问、给选项,让用户选择,问答追加到 `requirement.md`。不臆测、不隐藏困惑。
- **spec**:把原始需求转成结构化需求,定义**强且可验证**的验收标准;只描述 what,不含技术实现;多种解读并存时摆出来,不私自选定。
  - *spec-review(可选 subagent)*:检查需求是否完整、一致、可测、无歧义;意见回馈给 spec 步骤。
- **plan**:为 `spec.md` 设计技术方案。**简单优先**——先问「资深工程师会不会觉得过度设计」;复用既有模式,不为假想未来留扩展;非平凡取舍可深思,但写下来要精炼。
  - *plan-review(可选 subagent)*:检查方案的正确性、设计契合度、风险与更简替代;意见回馈给 plan 步骤。
- **develop**:实现 `plan.md`,**外科手术式改动**——只动任务所需,匹配既有风格;写/更新测试并跑通,跑 lint;同步更新受影响的项目级文档(如 project-level spec)。确保**完整交付**:开发、测试、文档三者齐备,不单设 doc-sync 步骤。
  - *develop-review(可选 subagent)*:对照 `plan.md` review 代码 diff,关注正确性、安全、并发、错误处理与测试覆盖;意见回馈给 develop 步骤。

## Resume Support

启动时若 `<session-dir>` 已有部分产物,读取现有 `requirement.md` / `spec.md` / `plan.md`,从第一个缺失的主干步骤继续;已完成的步骤不重复。
