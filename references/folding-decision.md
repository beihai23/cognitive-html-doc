# 折叠合法性判定

## 核心原则

**折叠 ≠ 次要**。

折叠是为了"按需获取"，不是"为了简洁而隐藏"。如果内容是核心架构或决策依据，应该展开，而不是折叠让用户必须点开才能看到。

## 三类内容判定

每段内容属于以下三类之一，决定呈现方式：

### 类型 1：核心 → 完全展开 + 视觉强化

**判定标准**（任一满足）：
- 删了之后逻辑链会断
- 是产品/技术决策的依据（"为什么这么做"）
- 是 Phase 1 必做项 / 红线 / 护城河
- 是核心架构组件

**呈现**：
- 完全展开，不折叠
- 视觉强化：
  - 关键卡片用 `border-2` + 状态色边框
  - 关键数字用 `text-3xl` 大字号
  - callout 前置到章节开头
  - 重要项用 ⭐ badge 标记

**例子**：
- 5 个理由（为什么选这个组合）
- 8 个核心模块（Phase 1 必做架构）
- 6 条红线
- 反馈循环（护城河来源）

### 类型 2：重要但按需查阅 → 折叠 + 强化引导

**判定标准**（任一满足）：
- 主线已表达，这是深入细节
- 决策溯源（之前讨论过的"为什么"，现在作为依据保留）
- 落地细节（工程/运营/法务需要，PM 不必读）
- 偶尔查阅的参考（术语表、引用关系）

**关键：折叠保留完整内容 ≠ 摘要。** 双层型模式（见 `references/fidelity-and-mapping.md`）的核心机制就是把规格层（字段定义、状态机、错误码、验收标准）折叠起来——但折叠的是**全文**，不是把字段表摘要成一句话。读者点开 `<details>` 应该看到完整规格，而不是"此处包含若干字段"。如果一份规格折叠后内容明显变短，说明被摘要了，违反了"执行规格 100% 保留"的保真门。

**呈现**：
- 折叠到 `<details>`
- **强化引导**（4 个元素）：

```html
<details class="card border-l-4 border-l-warning-border">
  <summary class="px-4 py-3 cursor-pointer font-medium flex items-center hover:bg-gray-50">
    <span class="summary-icon-closed mr-2 text-warning-text">▸</span>
    <span class="summary-icon-open mr-2 text-warning-text">▾</span>
    <span class="text-sm flex-1">标题</span>
    <span class="ml-auto flex items-center gap-2">
      <span class="badge bg-warning-bg text-warning text-[10px] border border-warning-border">
        🔒 决策依据
      </span>
      <span class="text-[10px] text-ink-500">4 个理由</span>
    </span>
  </summary>
  <!-- 内容 -->
</details>
```

**四个强化元素**：

| 元素 | 作用 |
|---|---|
| 左 4px 色条（`border-l-4 border-l-warning-border`） | 从灰色边缘变有色锚点 |
| 图标颜色匹配（`text-warning-text`） | summary 整体颜色一致 |
| 性质 badge（`🔒 决策依据` / `📋 工程运营必读` / `⚠️ 待决` / `📚 参考` / `📖 术语`） | 让读者知道"为什么该点开" |
| 内容计数（`4 个理由` / `5 项` / `10 个`） | 让读者知道"点开能得到什么" |

**色条颜色按性质**：
- warning 黄：决策依据、待决项、落地细节
- info 蓝：参考文档、术语表

### 类型 3：真次要 → 朴素折叠

**判定标准**（任一满足）：
- 几乎不看（变更日志、归档历史）
- 极少查阅的元数据
- 占空间但无重要信息

**呈现**：
- 折叠到 `<details>`
- 朴素 summary（无色条、无 badge）

```html
<details class="card">
  <summary class="px-4 py-3 cursor-pointer text-sm font-medium flex items-center hover:bg-gray-50">
    <span class="summary-icon-closed mr-2 text-ink-500">▸</span>
    <span class="summary-icon-open mr-2 text-ink-500">▾</span>
    附录 C：变更日志
  </summary>
  <!-- 内容 -->
</details>
```

## 判定流程图

```
判断内容 →
├── 删了逻辑链会断？
│   └── YES → 类型 1（完全展开 + 视觉强化）
├── 主线已表达，是深入细节 / 决策溯源 / 落地细节？
│   └── YES → 类型 2（折叠 + 强化引导）
└── 几乎不看 / 元数据？
    └── YES → 类型 3（朴素折叠）
```

## 默认 open 的判断

有时候类型 2 的内容希望首屏就能看到（比如"5 个未决项"——团队必须知道）。这种情况：

```html
<details open class="card border-l-4 border-l-warning-border">
  ...
</details>
```

加 `open` 属性默认展开，但仍保留折叠能力（用户可以收起）。

**何时用 `open`**：
- 未决项（团队必须看见）
- 当前阶段的关键约束
- 风险列表（高严重度）

**何时不用**：
- 决策溯源（之前讨论过的，现在作为依据保留）
- 落地细节（按需查阅）
- 术语表、参考文档

## 常见错误

### 错误 1：为简洁折叠核心

**错误**：
```html
<details>
  <summary>5 个理由</summary>
  <!-- 决策依据 -->
</details>
```

**问题**：5 个理由是"为什么选这个组合"的核心依据，折叠后用户大概率不会点开，决策依据丢失。

**修复**：完全展开，作为主线卡片。

### 错误 2：朴素 summary 用于重要内容

**错误**：
```html
<details>
  <summary>详细规范</summary>
  <!-- 工程师必读 -->
</details>
```

**问题**：summary 没有任何视觉引导，用户不知道"详细规范"是必读还是可跳过。

**修复**：加色条 + badge + 计数：
```html
<details class="card border-l-4 border-l-warning-border">
  <summary class="...">
    <span class="text-sm flex-1">详细规范</span>
    <span class="badge ...">📋 工程必读</span>
    <span class="text-[10px] text-ink-500">12 项</span>
  </summary>
</details>
```

### 错误 3：把多个不同性质的内容塞同一个 details

**错误**：把"详细画像 + 5 个理由 + 排除项"全塞一个折叠里。

**问题**：5 个理由是核心（应该展开），详细画像是落地（可以折叠）。

**修复**：分开展开/折叠。核心内容提到主线，落地细节单独折叠。

## 自检清单

每处折叠，问自己：

- [ ] 这个内容是核心架构/决策依据吗？→ 应该展开
- [ ] 如果展开会让排版多乱？→ 用次一级视觉权重（紧凑卡片）而不是折叠
- [ ] summary 有色条 + badge + 计数吗？（类型 2 必须）
- [ ] badge 性质描述准确吗？（决策依据/必读/参考）
- [ ] 计数和实际内容数量一致吗？

## 反例对照

| 内容 | 错误折叠 | 正确处理 |
|---|---|---|
| 5 个理由 | 折叠 | **展开**（决策依据） |
| 8 个核心模块 | 折叠 | **展开**（核心架构） |
| 详细用户画像 | 朴素折叠 | **强化引导折叠**（落地细节） |
| 术语表 | 朴素折叠 | **强化引导折叠**（带"10 个"计数） |
| 变更日志 | 朴素折叠 | **朴素折叠**（真次要，正确） |
| 未决项 | 朴素折叠 | **强化引导 + 默认 open**（团队必看） |
