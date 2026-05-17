---
name: step-8-ppt
description: 麦肯锡七步法第 8 步（可选交付物）：把 step-7 建议书翻译成商业战略汇报 PPT。当 step-7 已落盘并需要面向决策者做正式汇报时使用。产出 docs/strategy/<code>/<code>-report.pptx，使用 python-pptx，遵守版式规格（默认：微软雅黑 + 白底 + 主色 #1E53A4 蓝 + 中性灰辅助 + #D80C18 红强调），用户可改写主题色与字体。
---

# 第 8 步：制作汇报 PPT（Build Strategy Deck）

<HARD-GATE>
进入本步骤前必须满足：
- 存在 `docs/strategy/<code>/step-7-recommendation.md`
- step-7 自检清单已通过（执行摘要可独立成篇 + 每阶段有决策门 + 风险有早期信号 + Next 7 Days 具体）

未满足 → 拒绝执行，回退到 step-7。
</HARD-GATE>

## 本步骤的目标

把 step-7 的建议书翻译成 **可独立汇报的 PPT**，给决策者在会议中走查。
PPT 是 step-7 的视觉投影，**不引入新内容**——所有数字、决策门、风险都来自 step-7。

**完成的标准**：
- `docs/strategy/<code>/<code>-report.pptx` 落盘
- 14 ± 2 页（覆盖封面 / 议程 / SCP / 三大理由 / 三大论点 / 不建议清单 / 实施路径 / 资源 / 风险 / KPI / Next 7 Days / 决策请求）
- 字体、配色、版式遵守 `ppt-spec.md`（或用户自定义后的版本）
- 可在 PowerPoint / Keynote / WPS 中正常打开，中文字体不丢字

## 方法论核心

1. **PPT 是建议书的视觉投影,不是再创作**：每页核心内容必须能在 step-7 中找到出处；不出新数据
2. **结论先行 + 一页一论点**：每页只回答一个问题,标题就是结论;副标题是判断依据
3. **可读优先于美观**：14 ± 2 页是给决策者看的,不是给设计师评奖;数字、风险阈值、决策门必须清晰可读
4. **版式刚性,内容柔性**：颜色 / 字体 / 边距 / 字号档位是规格(默认见 `ppt-spec.md`),内容可按项目调整
5. **可重新生成**：脚本驱动(python-pptx),内容改了重跑即可——不要在 PPT 里手改

## 引导式对话流程

### Step 8.1 — 复制脚本与规格

把本 skill 同目录下的两份文件复制到项目：

| 源 | 目标 |
| --- | --- |
| `build_ppt_template.py` | `docs/strategy/<code>/build_ppt.py` |
| `ppt-spec.md` | `docs/strategy/<code>/ppt-spec.md`（仅在用户要改默认规格时复制） |

确认 `python-pptx` 可用：

```bash
python3 -c "import pptx; print(pptx.__version__)"
```

如缺，安装（macOS PEP 668 环境下加 `--break-system-packages`）：

```bash
pip3 install --user --break-system-packages python-pptx
```

### Step 8.2 — 确认版式规格

向用户输出默认规格并问是否需要改：

> 默认 PPT 版式（见 `ppt-spec.md`）：
>
> - **字体**：微软雅黑 (Microsoft YaHei,中英文统一)
> - **背景**：白色
> - **主色调**：蓝色 #1E53A4
> - **辅助色**：中性灰(深 #404040 / 中 #7F7F7F / 浅 #E6E6E6)
> - **强调色**：红色 #D80C18(用于关键数字、风险高亮、HIGHLIGHT 数据点)
> - **版式**：16:9 宽屏(13.333" × 7.5")
> - **字号档位**：标题 28pt / 副标题 14pt / 正文 11pt / 注脚 9pt
>
> 是否需要调整？

如需调整,改 `build_ppt.py` 顶部的 `THEME` / `FONT_*` 常量,而不是去 PPT 里手改。

### Step 8.3 — 加载 step-7 内容,逐页填内容

读取 `step-7-recommendation.md`,把 14 页(或用户调整后的页数)按以下默认结构对应回 step-7 章节：

| # | 页面 | 取自 step-7 章节 |
| --- | --- | --- |
| 1 | 封面 | 标题 + 元信息(收件人 / 日期) |
| 2 | 议程 | step-7 总目录 |
| 3 | 执行摘要(SCP 三栏) | §1 SCP 结构 |
| 4 | 三大理由 + 三个关键数字 | §1 三大理由 + 关键数字 |
| 5 | 市场分层(三市场列对比) | §2.1 表第 1 行 + 论点 A |
| 6 | 产品节奏(V1 → V1.5 + 护城河) | §2.1 第 2-4 行 + 论点 B |
| 7 | 增长引擎接力(CAC + 时间线) | 论点 C + §2.1 增长引擎行 |
| 8 | 我们不建议什么 | §2.2 全表 |
| 9 | 实施路径(4 阶段决策门) | §3 全表 |
| 10 | 资源需求(分阶段) | §4 全表 |
| 11 | 风险与早期信号 | §5 全表 |
| 12 | KPI 矩阵(4 维度) | §6 全表 |
| 13 | Next 7 Days | §7 全表 |
| 14 | 决策请求 | §1 P 部分 + §3 决策门 |

**关键原则**：每页**只放 step-7 已落盘的内容**。如果发现 step-7 缺数据,**回 step-7 补**,不要在 PPT 里编。

### Step 8.4 — 用脚本生成 PPT

用户(或本 skill 协助)在 `build_ppt.py` 中填好每页内容后:

```bash
cd docs/strategy/<code>
python3 build_ppt.py
```

输出: `<code>-report.pptx`(同目录)。

### Step 8.5 — 字符串内引号陷阱(强制提示)

python-pptx 内嵌中文字符串里若出现成对中文双引号 `"…"` 会被 Python 解析器当作字符串结束。**所有出现在字符串字面量中的成对双引号必须改成 `「」` 或 `《》`**。

每次写完一段内容,跑一次 `python3 -c "import ast; ast.parse(open('build_ppt.py').read())"` 验证语法。

### Step 8.6 — 中文字体绑定(强制实现)

python-pptx 的 `font.name = "Microsoft YaHei"` **只设 Latin face**,不影响中文渲染——中文会回退到默认宋体或方正等。
必须在每个 run 的 `<a:rPr>` 里手动添加 `<a:ea typeface="Microsoft YaHei"/>` 元素。
模板里 `set_run()` 已经实现,**复用即可,不要在新代码里绕过**。

### Step 8.7 — 验证产物

生成后检查:

- [ ] 文件落盘且大小合理(14 页 ≈ 50-100 KB,带图表更大)
- [ ] 用 PowerPoint / Keynote / WPS 打开任一种,中文不丢字
- [ ] 主色与强调色与规格一致
- [ ] 关键数字(67-90% / $300M+ / $1000-3000 万 一类)与 step-7 一致

### Step 8.8 — 可选:配合口头汇报

提醒用户:

> 这份 PPT 是给决策者**走查用的草稿**——按 §3 表的 4 阶段决策门走一遍 + 念一遍 Next 7 Days,看决策者在哪一页皱眉头。
> ta 的反应会暴露 step-7 里没说清楚的地方,回去修 step-7 的对应章节,然后**重跑脚本**,不要直接改 pptx。

### Step 8.9 — 走自检

对照本 skill "自检清单"逐项过。

## 产出物

- 模板:本 skill 同目录 `build_ppt_template.py` + `ppt-spec.md`
- 落盘:
  - `docs/strategy/<code>/build_ppt.py`(可重跑的脚本)
  - `docs/strategy/<code>/<code>-report.pptx`(交付给决策者的 PPT)

## 自检清单(完成门禁)

- [ ] PPT 文件已生成,可正常打开
- [ ] 14 ± 2 页,覆盖默认页面结构
- [ ] 中文字体生效(微软雅黑,中英统一)
- [ ] 主色 / 强调色 / 灰阶与规格一致
- [ ] 每页内容都能追溯到 step-7 出处
- [ ] PPT 没有 step-7 没有的新数字 / 新结论
- [ ] `build_ppt.py` 留在项目目录,内容可改可重跑

## 反模式(红旗)

| 红旗信号 | 真相 |
| --- | --- |
| 在 PPT 里直接手改内容 | 失去可重生成性,下次同步 step-7 改动会丢 |
| 引入 step-7 没有的新数字 | PPT 是投影不是再创作,出新内容回 step-7 改 |
| 一页堆 5+ 论点 | 一页一论点;再多就拆页或放进备注 |
| 用图片代替数据表 | 决策者要看具体数字,图片不可读会挨问 |
| 中文回退到宋体 | 没设置 East Asian font(`<a:ea>`),用模板的 `set_run` |
| 字符串内放成对中文双引号 | Python 语法错误,改成「」/《》 |
| 字号 ≤ 8pt | 投影不可读,守 9pt 注脚下限 |
| 跳过 step-7 直接做 PPT | 没结论的 PPT 是装饰;先把 step-7 跑完 |

## 完成后交接(项目最终收尾)

向用户输出:

> ✅ Step-8 完成。PPT 已生成: `docs/strategy/<code>/<code>-report.pptx`
>
> 麦肯锡七步成诗(扩展版,含 PPT)全流程已走完。`docs/strategy/<code>/` 下产出物:
> - step-1 ~ step-7 全部 markdown
> - step-5-evidence.md 佐证档
> - build_ppt.py + <code>-report.pptx ← 拿这份去汇报
>
> 接下来:
> 1. 把 PPT 当面过给决策者(走 §3 决策门 + Next 7 Days)
> 2. 根据反馈回到对应 step 修订 markdown,重跑 `build_ppt.py`
> 3. 进入实施阶段,按 Next 7 Days 推进

更新 `docs/strategy/<code>/README.md` 的"七步导航"加一行:

```
- [x] step-8: 汇报 PPT → `<code>-report.pptx`
```

附加日志:

```
- YYYY-MM-DD: step-8 完成,商业战略汇报 PPT 已生成
```
