---
name: cognitive-html-doc
description: 将密集、线性的 Markdown 技术/产品文档重构为认知降维的工业级单文件 HTML 文档。核心目标是让读者 3 秒抓核心、30 秒理解全貌、3 分钟查到细节。使用此 skill 当用户：把 markdown 转成 HTML、要求做"漂亮的 HTML 文档"、要求"工业级 HTML"、要"技术文档可视化"、给一份 markdown 蓝图要 HTML 化、需要带 TOC/Mermaid 图表/卡片设计的长文档、提到"降低认知负荷"或"扫视即可获取"。即使没明说"HTML 文档"，只要涉及把密集文字降维成结构化、可扫读的形态，就用此 skill。不要用于简单 markdown 渲染（一行命令即可）或纯打印样式 PDF。
---

# Cognitive HTML Doc

把密集的 Markdown 重构为**认知降维**的工业级 HTML 文档。

## 核心哲学

Markdown 的本质是纯文本，长篇阅读认知负荷极高。这个 skill 的目标是**打破平铺直叙**，利用 HTML 的空间布局、交互组件、图形化能力，把信息降维成"扫视即可获取"的结构。

读者阅读技术文档时分三种节奏，产出必须同时满足：

- **3 秒抓核心**：Hero 区一句话定位 + 4 维度卡片 → 整个产品/系统是什么、给谁、怎么实现、衡量什么
- **30 秒理解全貌**：主架构图（颜色编码）→ 系统拓扑一目了然
- **3 分钟查细节**：固定侧边栏 TOC + 滚动高亮 → 直达目标章节

## 何时触发

| 触发场景 | 例子 |
|---|---|
| 明确要求 HTML 化 | "把这份 markdown 转成 HTML"、"做一份工业级 HTML 文档" |
| 要求视觉重构 | "这个 markdown 太长了，能可视化吗"、"降维成扫视即可获取的结构" |
| 提供蓝图 + 期望高级呈现 | 用户给出 markdown + 提示词要求"高可读性、零依赖、单文件" |
| 长文档工程化 | 文档超过 500 行，需要 TOC、章节锚点、状态色系统 |

**不要用于**：一行命令就能搞定的 markdown 渲染（如 `pandoc`）；纯打印 PDF；不需要交互的简单文档。

## 工作流程（5 步）

### Step 1 · 通读 Markdown，画逻辑链

不要直接开始写 HTML。先通读全文，画出**核心逻辑链**：

```
输入（数据/触发） → 处理 → 输出 → 反馈/沉淀
```

这一步决定后续每节内容是"逻辑链上的哪一环"。**逻辑链断裂的章节就是要补的关键环节**（见 Step 2）。

### Step 2 · 检查逻辑完整性（最重要）

逐章问自己：**这章删了，逻辑链会不会断？**

如果会断，就是核心章节，不能省略，不能折叠。
如果用户给的 markdown 缺了关键环节（比如产品文档没有数据来源、技术方案没有错误处理），**必须主动指出并补上**——不能以"原文没写"或"这是技术细节"为由跳过。

典型容易遗漏的关键环节：
- 产品文档：**数据来源**、关键约束的依据、未决项
- 技术方案：**错误处理**、回滚策略、监控告警
- API 文档：**鉴权**、限流、错误码

详见 `references/completeness-check.md`。

### Step 3 · 设计信息分层

每个章节的内容分三层，决定呈现方式：

| 层级 | 判定 | 呈现方式 |
|---|---|---|
| **核心** | 删了逻辑链会断 / 决策依据 / Phase 1 必做项 | **完全展开**，视觉强化（KPI 大字号、状态色边框、Hero 卡片） |
| **重要但按需查阅** | 主线已表达，按需深入（详细配置、子案例、决策溯源） | 折叠 + **强化引导**（色条 + badge + 内容计数） |
| **真次要** | 几乎不看（变更日志、归档） | 朴素折叠 |

**关键判定**：折叠 ≠ 次要。**"为了简洁而折叠"是常见错误**——如果内容是核心架构或决策依据，应该展开，而不是折叠让用户点开才能看到。

详见 `references/folding-decision.md`。

### Step 4 · 套视觉锚点

按内容类型选合适的表现形式：

| 内容类型 | 表现形式 |
|---|---|
| 关键数字（占比、目标、规模） | **KPI 大字号卡片**（脱离表格，用 `tabular-nums`） |
| 核心约束 / 警告 | **callout**（左 4px 色条 + 浅色背景，前置到章节开头） |
| 系统架构 | **Mermaid graph** + subgraph 分层 + classDef 颜色编码 |
| 业务流程 | **Mermaid flowchart** + 决策分支色分 |
| 时序交互 | **Mermaid sequenceDiagram** + autonumber + Note |
| 状态机 | **Mermaid stateDiagram-v2** |
| 时间线 / 阶段演进 | **Mermaid timeline** |
| 多人群 / 多方案对比 | **并排卡片**（不同颜色边框）+ Tab 切换 |
| 长列表 / 复杂层级 | **嵌套卡片网格** 或 **SVG 自定义可视化**（如金字塔、Venn 图） |
| 详细参数 / 配置 | **响应式表格**（sticky header、斑马线、hover 高亮） |
| 术语 | 折叠到附录，正文用 `<abbr>` 或 tooltip 内联 |
| API 字段 | 卡片 + 字段说明 |

详见 `references/visualization-patterns.md`。

### Step 5 · 系统扫描每章（写完后必做）

写完 HTML 后**逐章扫描**，对每章问：

1. **核心论点是什么？**（用一句话总结）
2. **当前设计是否让这论点成为视觉重点？**
3. **是否有信息平铺、缺少视觉层级？**

**不要只盯用户指出的单点**。如果用户指出一处问题，主动扫描全文找类似的——常见的同类问题：
- 表格里关键行淹没在普通行中
- 多卡片网格中所有卡片同等视觉权重（没突出关键）
- 数字藏在文字段落里（应该 KPI 化）
- 折叠内容没有强化引导（朴素 summary，用户不会点开）

## 视觉系统

### 状态色（4 态，全局一致）

| 状态 | 用途 | Tailwind 配色 |
|---|---|---|
| **success**（绿） | 可做 / 已完成 / 推荐项 | `text-success` `border-success` `bg-success` |
| **warning**（黄） | 部分支持 / 待决 / 警告 | `text-warning` `border-warning` `bg-warning` |
| **danger**（红） | 禁止 / 红线 / 失败 | `text-danger` `border-danger` `bg-danger` |
| **info**（蓝） | 信息 / 引用 / 提示 | `text-info` `border-info` `bg-info` |

每色三态：text（文字）/ border（边框）/ bg（背景，加 `/20` `/30` 控制透明度）

### 卡片设计

- 圆角 8px，subtle border (`border` 色)
- hover 时 `translateY(-1px)` + shadow 加深
- 关键卡片用 `border-2`（2px 加粗边框）
- KPI 数字用 `text-3xl` 以上 + `font-bold` + `tabular-nums`

### Callout

左 4px 色条 + 浅色背景 + 1px 边框。用于：
- `info`（蓝）：补充说明、引用其他文档
- `success`（绿）：核心论点、最佳实践
- `warning`（黄）：注意事项、Phase 边界
- `danger`（红）：红线、必做约束、安全机制

### 折叠 details 强化引导

如果 details 包含"重要但按需查阅"的内容，summary 必须强化：

```html
<details class="card border-l-4 border-l-warning-border">
  <summary class="px-4 py-3 cursor-pointer font-medium flex items-center hover:bg-gray-50">
    <span class="summary-icon-closed mr-2 text-warning-text">▸</span>
    <span class="summary-icon-open mr-2 text-warning-text">▾</span>
    <span class="text-sm flex-1">标题</span>
    <span class="ml-auto flex items-center gap-2">
      <span class="badge bg-warning-bg text-warning text-[10px] border border-warning-border">🔒 决策依据</span>
      <span class="text-[10px] text-ink-500">4 个理由</span>
    </span>
  </summary>
  <!-- 内容 -->
</details>
```

四个强化元素：色条 / 图标颜色 / badge / 内容计数。

## CDN 栈（推荐）

```html
<!-- Tailwind CSS（含自定义 theme） -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- Highlight.js -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/github.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js"></script>
<!-- Mermaid.js -->
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<!-- Alpine.js（Tab/Tooltip 等轻交互） -->
<script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
```

**注意**：Tailwind CDN 必须配 `tailwind.config` 自定义 theme（颜色、字体），不能裸用默认色。

详见 `references/cdn-stack.md`。

## 完整性检查清单（交付前必过）

写完 HTML 后，过这个清单：

- [ ] **逻辑链条无断点**：每章都是逻辑链上的环节，关键环节都补齐（数据来源、错误处理、回滚、监控等）
- [ ] **每章核心论点突出**：扫读时第一眼能看到核心信息，不是埋在段落中间
- [ ] **折叠合法性**：折叠的内容是"真次要"或"重要但按需查阅"，不是"为了简洁而藏起来的核心信息"
- [ ] **折叠引导**：所有折叠都有色条 + badge + 内容计数（真次要除外）
- [ ] **KPI 数字脱离表格**：占比、目标、规模等数字用大字号卡片呈现，不在表格里
- [ ] **关键约束前置**：每章第一屏就能看到核心约束，不必读完才理解
- [ ] **状态色一致**：success/warning/danger/info 全局统一用法
- [ ] **Mermaid 图正确渲染**：所有 mermaid 块都有 `mermaid.initialize()` 配置
- [ ] **TOC 同步**：侧边栏 TOC 链接和章节 id 一一对应
- [ ] **多载体同步**：如果改了源 markdown，衍生 HTML 同步更新；反之亦然

## 详见 references/

- `references/cdn-stack.md` — CDN 详细配置（Tailwind theme / Mermaid initialize / Alpine 用法）
- `references/visualization-patterns.md` — 内容→图表类型映射 + Mermaid 模式库
- `references/folding-decision.md` — 折叠合法性判定流程图
- `references/completeness-check.md` — 逻辑完整性检查清单（按文档类型）

## assets/

- `assets/template.html` — HTML 骨架模板（可直接拷贝改造）
- `assets/components.md` — 常用组件示例代码（Hero / KPI 卡片 / callout / 强化 details / 状态色 / Tab / 表格）

## 常见反模式（避免）

1. **"Markdown + CSS"伪重构**：只换字体颜色和加表格斑马线，没做空间重组 → 不算认知降维
2. **ASCII 图直接搬运**：架构图、流程图仍然用 ASCII 代码块 → 应该转 Mermaid 或嵌套卡片
3. **Tab 隐藏核心对比**：双人群/双方案对比用 Tab 切换，首屏看不到 → 应该并排卡片
4. **为了简洁折叠核心**：把"5 个理由"或"8 个模块"折叠让排版更清爽 → 应该展开
5. **朴素 summary**：折叠的 summary 只有标题，没色条没 badge → 用户不会点开
6. **数字藏在文字里**：关键 KPI 写在段落中间 → 应该 KPI 卡片大字号
7. **逻辑链断点**：跳过"数据来源"或"错误处理"等关键环节 → 逻辑不成立
8. **只盯用户指出的单点**：用户指出一处问题，不扫描全文找同类 → 系统性问题漏诊
