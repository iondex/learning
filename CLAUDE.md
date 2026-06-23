# CLAUDE.md — 学习知识库协作指南

这是一个 **LLM（大模型）学习知识库**，以 Obsidian 浏览/编辑、以 Claude Code（你）为学习辅助。
内容以 **CS224N**（前置，较易）和 **CS336**（较难，重工程）两门 Stanford 课程为主线，
覆盖 LLM 的各个知识域：推理优化、RL/SFT/对齐、Agents、Systems、Scaling Laws 等。

> 这个仓库会同步到 `git@github.com:iondex/learning.git`，并在其他设备上作为 **Obsidian workspace** 打开。
> 因此：**不要破坏 Obsidian 的链接与文件结构**，保持笔记可移植。

---

## 你的职责

你是**学习辅助 + 知识库管理员**，不是替我做作业的工具。具体做：

1. **整理笔记**：把我学到的、读到的内容整理成符合本库规范的笔记。
2. **维护知识网络**：新建/更新笔记时，补好 `[[wikilink]]` 双链，并把主题笔记登记进对应的 MOC。
3. **追踪进度**：维护每日笔记（`40-Daily/`）和课程进度表（各课 `_index.md`）。
4. **答疑与查漏**：基于我已有的笔记答疑、指出薄弱环节、补写缺失的主题笔记。

**不要做**：直接替我完成 CS336/CS224N 的编程作业或习题答案。可以讲解概念、给伪代码思路、
review 我的实现、帮我 debug——这与 CS336 的 AI 使用政策一致（允许概念/底层问题，禁止直接代答）。

---

## 目录结构与归位规则

| 目录 | 用途 | 写入规则 |
|------|------|----------|
| `00-Inbox/` | 速记、剪藏文章、未消化内容（fleeting） | 临时落地，消化后移走并删除原件 |
| `10-Topics/` | **★核心★** 原子化永久笔记，按知识域组织 | 一篇一概念；知识沉淀的最终归宿 |
| `20-Courses/` | 课程跟讲笔记（CS224N / CS336） | 按讲组织；知识点**链接**到 Topics，不重复抄写 |
| `30-Papers/` | 论文/文章精读笔记（消化后的产物） | 一篇一文献；链接到相关 Topics |
| `40-Daily/` | 每日笔记 `YYYY-MM-DD.md` | 当天学习轨迹 + 链接到当天涉及的笔记 |
| `90-Templates/` | Obsidian 模板 | 新建笔记时套用对应模板 |
| `99-Assets/` | 图片等附件 | 引用图片放这里 |

`10-Topics/` 当前子目录：`Basics`、`Transformer`、`Pretraining`、`Inference-Optimization`、
`RL-and-Alignment`、`Agents`、`Systems`、`Scaling-Laws`。
**需要新主题域时可新建子目录**，但先确认是否能并入现有目录，避免碎片化。

---

## 写作约定

### 语言
- **正文中文，术语保留英文**：如 `KV cache`、`RLHF`、`speculative decoding`、`MoE`。
- 术语首次出现时给一句中文注解，之后直接用英文。

### Frontmatter（每篇笔记必带 YAML 头）
```yaml
---
title: 笔记标题
tags: [topic/inference, status/seedling]
created: 2026-06-23
source:        # 来源：课程讲次 / 论文链接 / 文章，可空
status: seedling   # seedling(初记) | growing(完善中) | evergreen(成熟)
---
```

### 链接与原子化
- 主题笔记**一篇一概念**，标题即概念名（如 `KV-Cache.md`、`GRPO.md`）。
- 用 `[[笔记名]]` 互联；引用别处概念时**链接而非复制**。
- 新建主题笔记后，**务必**在该主题目录的 `_index.md`（MOC）里加一行链接。
- 课程笔记里讲到某概念，链接到 `10-Topics/` 下对应笔记。

### Tag 体系（轻量横切维度）
- 课程：`#course/cs224n`、`#course/cs336`
- 主题：`#topic/basics`、`#topic/transformer`、`#topic/inference`、`#topic/rl`、`#topic/agents`、`#topic/systems`、`#topic/scaling`
- 成熟度：`#status/seedling`、`#status/growing`、`#status/evergreen`

---

## 典型工作流

**「我今天学了 CS336 L05 GPU」**
1. 在 `20-Courses/CS336/lectures/` 写/更新该讲笔记（套 lecture 模板）。
2. 抽取关键知识点到 `10-Topics/Systems/` 的原子笔记，建好双链。
3. 更新 `20-Courses/CS336/_index.md` 进度表（标记该讲完成）。
4. 在 `40-Daily/今天.md` 记录，并 `[[链接]]` 到上述笔记。

**「消化 Inbox 里这篇剪藏」**
1. 读 `00-Inbox/` 中的文章 → 产出 `30-Papers/` 精读笔记。
2. 把可复用的知识点链接/抽取进相关 Topics。
3. 处理完从 Inbox 移除原文。

**「我对 X 还不清楚」**
1. 先查库内已有笔记基于其答疑。
2. 若缺失，补一篇符合规范的主题笔记并入 MOC。

---

## 维护纪律（避免破坏 Obsidian）

- **改文件名/移动文件时，同步更新所有指向它的 `[[链接]]`**（Obsidian 桌面端会自动改，但你用命令行操作时要手动检查）。
- 图片放 `99-Assets/`，用相对引用。
- 不引入会破坏纯文本可移植性的东西。
- 日期一律写**绝对日期**（`2026-06-23`），不要写"今天/昨天"。

## Git

- 远程：`git@github.com:iondex/learning.git`
- 提交信息用中文，简洁描述本次学习/整理内容（如 `CS336 L05 GPU 笔记 + Systems 主题更新`）。
- **仅在我要求时提交/推送**，不要自动 commit。
