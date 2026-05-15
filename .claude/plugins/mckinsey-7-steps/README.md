# 麦肯锡七步成诗战略分析 Plugin

把麦肯锡经典的「七步成诗」战略分析方法论沉淀为一组 Claude Code Skill，通过引导式对话，帮助你从一个模糊的 idea 走到一份可执行的行动方案。

## 启动方式

在本项目工作目录下，对 Claude 说：

> 我想用麦肯锡七步法分析 XXX

Claude 会自动调用 `mckinsey-strategy` 总入口 skill，引导你命名项目代号，然后依序进入第 1 步到第 7 步。

也可以直接输入 `/mckinsey-strategy` 显式触发。

## 流程概览

```
idea
 │
 ▼
┌──────────────────────────────────────────┐
│ mckinsey-strategy （总入口：初始化 + 导航）│
└──────────────────────────────────────────┘
 │
 ▼
1. step-1-define-problem      陈述问题（SCQA + 决策者卡）
 │
 ▼
2. step-2-structure-problem   分解问题（MECE 议题树 + 假设）
 │
 ▼
3. step-3-prioritize-issues   优先排序（影响×可解性，80/20）
 │
 ▼
4. step-4-workplan            制定工作计划（假设-分析-数据-责任）
 │
 ▼
5. step-5-conduct-analysis    进行关键分析（先粗后细，证伪）
 │
 ▼
6. step-6-synthesize          综合调查结果（金字塔，So What）
 │
 ▼
7. step-7-recommend           提出建议（SCP + 行动路径 + 风险）
 │
 ▼
docs/strategy/<项目代号>/  （7 份产出物 + README）
```

## 产出物位置

所有用户产出物落到当前工作目录的：

```
docs/strategy/<项目代号>/
├── README.md                  # 项目背景 + 7 步导航
├── step-1-problem.md
├── step-2-tree.md
├── step-3-priorities.md
├── step-4-workplan.md
├── step-5-analysis.md
├── step-6-synthesis.md
└── step-7-recommendation.md
```

## 设计原则

- **硬门禁（HARD-GATE）**：第 N 步必须读到第 N-1 步的产出文件，否则拒绝执行，强制回补
- **一次只问一题**：每个 skill 用 `AskUserQuestion` 启发式提问，避免一次抛 5 个问题
- **当场落盘**：用户回答后立刻增量写入产出文件，让结构在对话中生长
- **自检 + 反模式**：每步结束前走完自检清单，并对照「红旗 → 真相」表识别捷径

## 目录结构

```
.claude/plugins/mckinsey-7-steps/
├── .claude-plugin/plugin.json
├── README.md
├── skills/
│   ├── mckinsey-strategy/SKILL.md
│   ├── step-1-define-problem/SKILL.md
│   ├── step-2-structure-problem/SKILL.md
│   ├── step-3-prioritize-issues/SKILL.md
│   ├── step-4-workplan/SKILL.md
│   ├── step-5-conduct-analysis/SKILL.md
│   ├── step-6-synthesize/SKILL.md
│   └── step-7-recommend/SKILL.md
└── templates/
    ├── 01-problem-statement.md
    ├── 02-issue-tree.md
    ├── 03-prioritization-matrix.md
    ├── 04-workplan.md
    ├── 05-analysis-log.md
    ├── 06-synthesis-pyramid.md
    └── 07-recommendation.md
```
