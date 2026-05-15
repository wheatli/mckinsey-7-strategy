# McKinsey-7-Strategy

把麦肯锡经典的「**七步成诗**」战略分析方法论沉淀为一组 Claude Code Skills，通过引导式/启发式对话，帮助你从一个**模糊的 idea**，一步步走到一份**可执行的战略行动方案**。

> 这不是一份方法论文档，而是一个能"陪你走完全程"的 AI 教练。

---

## ✨ 这个项目解决什么问题

做战略分析时，最常见的失败模式是：

- 拿到问题就埋头算数据 → 拆解逻辑没想清，分析全跑偏
- 拆得很深但没量化 → 一堆"重要"、"显著"，没人能拍板
- 直接跳到解决方案 → 没回答"问题到底是什么"
- 自我证实而非证伪 → 所有假设都"成立"，红队反对找不到

本项目用 8 个 skill **强约束**整个流程：上一步没完成，下一步进不去；每步都有自检清单 + 反模式表，迫使你不能走捷径。

---

## 🗺️ 七步流程地图

```
idea
  │
  ▼
┌────────────────────────────────────────────┐
│ mckinsey-strategy（总入口：初始化 + 导航）  │
└────────────────────────────────────────────┘
  │
  ▼
1. step-1-define-problem      陈述问题（SCQA + 决策者卡）
  │
  ▼
2. step-2-structure-problem   分解问题（MECE 议题树）
  │
  ▼
3. step-3-prioritize-issues   优先排序（影响 × 可解性，80/20）
  │
  ▼
4. step-4-workplan            制定工作计划（假设-分析-数据-责任）
  │
  ▼
5. step-5-conduct-analysis    进行关键分析（先粗后细，证伪而非证实）
  │
  ▼
6. step-6-synthesize          综合调查结果（金字塔，主结论 + 3 论点）
  │
  ▼
7. step-7-recommend           提出建议（SCP + 行动路径 + 决策门）
  │
  ▼
docs/strategy/<项目代号>/  ← 7 份产出物 + README，可直接给决策者
```

---

## 🚀 快速开始

### 前置条件

- 安装 [Claude Code](https://claude.ai/code)
- 在本仓库的工作目录下使用（plugin 已注册到 `.claude/plugins/`，会被 Claude Code 自动识别）

### 启动方式

在本仓库目录里打开 Claude Code 会话，对 Claude 说：

```
我想用麦肯锡七步法分析「是否应该把团队从 5 人扩到 10 人」
```

或显式触发：

```
/mckinsey-strategy
```

Claude 会自动：

1. 调用 `mckinsey-strategy` 总入口 skill
2. 让你命名项目代号（kebab-case，如 `team-scale-5-to-10`）
3. 创建 `docs/strategy/<项目代号>/` 与 `README.md`
4. 引导进入 step-1，开始 SCQA 拆解

之后每完成一步，Claude 都会落盘一份 markdown，并提示是否进入下一步。

---

## 📂 产出物示例

完成一次完整分析后，你会得到：

```
docs/strategy/team-scale-5-to-10/
├── README.md                  # 项目背景 + 7 步导航 + 进度日志
├── step-1-problem.md          # 焦点问题陈述卡
├── step-2-tree.md             # MECE 议题树 + 假设清单
├── step-3-priorities.md       # 影响×可解性矩阵 + Key Drivers
├── step-4-workplan.md         # 分析工作计划
├── step-5-analysis.md         # 关键分析记录（含反向证据）
├── step-6-synthesis.md        # 金字塔结构 + 红队回应
└── step-7-recommendation.md   # 最终建议书 ← 拿这份给决策者
```

第 7 份文档**可独立成篇**——决策者无需读 step-1~6 也能看懂建议、风险、行动路径和 KPI。

---

## 📦 项目结构

```
McKinsey-7-Strategy/
├── README.md                                    ← 本文件（项目说明）
├── .claude/
│   └── plugins/
│       └── mckinsey-7-steps/                    ← Plugin 本体
│           ├── .claude-plugin/plugin.json       ← Plugin 元信息
│           ├── README.md                        ← Plugin 内部使用说明
│           ├── skills/                          ← 8 个 skill 定义
│           │   ├── mckinsey-strategy/SKILL.md   ← 总入口（流程协调器）
│           │   ├── step-1-define-problem/
│           │   ├── step-2-structure-problem/
│           │   ├── step-3-prioritize-issues/
│           │   ├── step-4-workplan/
│           │   ├── step-5-conduct-analysis/
│           │   ├── step-6-synthesize/
│           │   └── step-7-recommend/
│           └── templates/                       ← 7 份产出物模板
│               ├── 01-problem-statement.md
│               ├── 02-issue-tree.md
│               ├── 03-prioritization-matrix.md
│               ├── 04-workplan.md
│               ├── 05-analysis-log.md
│               ├── 06-synthesis-pyramid.md
│               └── 07-recommendation.md
└── docs/
    └── strategy/                                ← 用户产出物落盘位置
        └── <项目代号>/                           ← 每次分析一个子目录
```

---

## 🎯 设计原则

| 原则 | 体现 |
| --- | --- |
| **硬门禁（HARD-GATE）** | 第 N 步必须读到第 N-1 步的产出文件，否则拒绝执行 |
| **一次只问一个问题** | 用 `AskUserQuestion` 启发式提问，避免疲劳轰炸 |
| **当场落盘** | 每答完一题立即增量写入产出文件，结构在对话中生长 |
| **自检 + 反模式** | 每步结束前走自检清单，对照「红旗 → 真相」表识别捷径 |
| **证伪优先** | 强制为每个假设主动找反向证据，避免自我证实 |
| **结论先行** | 每个分析的输出必须是判断（"市场已饱和"），不是描述（"市场份额 60%"） |

---

## 🔑 适用场景

### 强烈推荐

- 业务战略决策（是否进入新市场、是否上新产品线）
- 组织决策（团队扩张 / 收缩、组织架构调整）
- 投资判断（项目立项、并购评估）
- 个人重大决策（换工作、买房、长期学习路径）

### 不太适合

- 显然单一选项的执行类任务（直接做就行）
- 纯创意发散（用 brainstorming 类 skill 更合适）
- 微小、高频、可逆的小决定（用轻量"口袋版三步"即可）

---

## 🪶 轻量级使用

不是每个问题都值得跑完 7 步。判断标准：

| 问题规模 | 建议 |
| --- | --- |
| 影响 < 1 个月、可逆 | 直接做，最多想想 SCQA |
| 影响 1-3 个月 | 跑 step-1 + step-2 + step-3 三步 |
| 影响 ≥ 1 个季度 / 不可逆 | 完整 7 步并落盘 |

口袋版三步（5+10+15 分钟）：
1. 写焦点问题 + SCQA（step-1 浓缩）
2. 画 MECE 树 + 标 Key Driver（step-2+3 浓缩）
3. 做 1 小时粗估，决定是否再深入

---

## 🛠️ 自定义与扩展

每个 skill 都是独立的 markdown 文件，结构清晰、易于修改：

- 想加一种拆解维度 → 改 `step-2-structure-problem/SKILL.md` 的引导对话
- 想换一种优先级矩阵（如 RICE）→ 改 `step-3-prioritize-issues/SKILL.md` + `templates/03-prioritization-matrix.md`
- 想加一个 step-8（实施跟踪） → 在 `skills/` 下加一个目录，并修改 step-7 的"完成后交接"

也可以直接 fork 这个仓库，按你的行业 / 团队习惯定制术语和模板。

---

## 📚 致敬

方法论原型来自麦肯锡公司的经典咨询流程，相关参考：

- 《金字塔原理》— Barbara Minto
- 《麦肯锡方法》— Ethan Rasiel
- 《麦肯锡问题分析与解决技巧》— 高杉尚孝

本项目把这套方法论"工程化"为可被 Claude Code 复用的 plugin，让 AI 真正能陪你做战略思考，而不只是回答问题。

---

## 📝 License

MIT
