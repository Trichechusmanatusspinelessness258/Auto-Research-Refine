# Research Refine Skill

把“研究问题已经比较明确，但方法路线还比较模糊”的阶段，推进成一份可执行、可评审、可落地的研究方案。

`research-refine` 会做三件事：

- 扫描本地论文和笔记，先建立 grounding
- 把模糊想法拆成清晰的研究问题、方法设计、实验方案和风险项
- 用 Claude + GPT-5.4 的外部 peer review 循环反复打磨，直到技术路线更清晰

## 适用场景

- 你已经有一个方向，但还没有清楚的方法设计
- 你知道要解决什么问题，但 baselines、datasets、metrics、ablations 还不完整
- 你想在项目初期先把技术路线和验证路径想透，再进入实现阶段

## 项目结构

```text
research-refine-skill/
├── README.md
├── .gitignore
├── research-refine/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
├── papers/
├── literature/
└── refine-logs/
```

## 安装

1. 把 skill 安装到 Claude Code：

```bash
cp -r research-refine ~/.claude/skills/
```

2. 如果你要用外部 reviewer 回路，先配置 Codex MCP：

```bash
npm install -g @openai/codex
claude mcp add codex -s user -- codex mcp-server
```

3. 把你的本地论文放进 `papers/`，把阅读笔记或摘要放进 `literature/`。

## 使用方式

在这个项目目录里启动 Claude Code，然后调用：

```bash
/research-refine "研究问题 | 初步方法"
```

示例：

```bash
/research-refine "视频生成奖励建模 | 先用偏好数据训练多目标 reward model，但 loss 设计、数据组织和评测方案还比较模糊"
```

```bash
/research-refine "test-time scaling for agent planning | 我想结合 verifier 和 search，但还没想清楚具体 pipeline、对比基线和 ablation"
```

## 输出内容

Skill 会把中间结果和最终方案写到 `refine-logs/`，典型文件包括：

- `round-0-initial-proposal.md`
- `round-1-review.md`
- `round-1-refinement.md`
- `score-history.md`
- `REFINEMENT_REPORT.md`

## 建议输入模板

为了让输出更好，建议你的输入至少包含：

- 研究问题：你到底想解决什么
- 初步方法：你目前最模糊但最关键的方案设想
- 约束条件：算力、数据、时间、目标 venue

一个简单模板：

```text
问题：
目前想法：
已有资源：
约束：
希望达到的结果：
```

## 上传 GitHub

如果你后面要把这个项目单独传到 GitHub，可以直接在这个目录执行：

```bash
git init
git add .
git commit -m "init research-refine skill"
```

如果你装了 GitHub CLI，可以继续：

```bash
gh repo create research-refine-skill --public --source=. --remote=origin --push
```

如果没有装 `gh`，就在 GitHub 网页上新建仓库后执行：

```bash
git remote add origin <your-repo-url>
git branch -M main
git push -u origin main
```

## 一句话定位

这是一个适合项目初期使用的 skill：当研究问题已经出现，但技术路线还没完全长出来时，它帮助你把“方向感”变成“执行方案”。
