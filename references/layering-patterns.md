# 信息分层模式库

## 核心原则

**信息分层 ≠ 折叠**。分层是更大的概念——决定"用户何时、如何看到什么信息"。折叠只是其中一种手段。

写文档时常常陷入"折叠 vs 展开"的二元对立，但实际上有 6 类手段，按需组合才是最优解。

## 6 类分层手段

| 手段 | 触发方式 | 信息长度 | 阅读成本 | 移动端 | 适用场景 |
|---|---|---|---|---|---|
| **空间位置** | 始终可见 | 任意 | 极低 | ✅ | 跨章节复用的约束 |
| **视觉权重** | 始终可见 | 短 | 极低 | ✅ | 突出关键数字/结论 |
| **Hover 悬浮** | 鼠标悬停 | 短 | 极低 | ❌ 不友好 | 术语、提示、引用摘要 |
| **折叠展开** | 点击 | 长 | 中 | ✅ | 完整论述、详细配置 |
| **跳转链接** | 点击 | 任意 | 中 | ✅ | 关联但分散的内容 |
| **附录归档** | 滚动到底部 | 任意 | 高 | ✅ | 几乎不看的元数据 |

## 1. 空间位置

**核心放主区，边缘放侧栏/边注。** 不改变内容可见性，只改变位置权重。

### 侧栏固定卡片（跨章节复用）

适用：全局红线、关键约束、术语提示（用户在任何章节都可能需要）

```html
<aside class="doc-sidebar">
  <!-- TOC -->
  <nav>...</nav>

  <!-- 关键约束卡片（跨章节复用） -->
  <div class="mt-8 pt-5 border-t border-line">
    <div class="text-[11px] uppercase tracking-wider text-ink-500 font-semibold mb-2">
      关键约束
    </div>
    <div class="space-y-2 text-[11.5px] text-ink-700">
      <div class="flex items-start gap-1.5">
        <span class="text-danger-text font-bold">·</span>
        <span>花钱永不代做</span>
      </div>
      <!-- 更多 -->
    </div>
  </div>
</aside>
```

### 章节内边注（单章节相关）

适用：章节特定的注释、警示、引用

```html
<div class="grid grid-cols-3 gap-4">
  <div class="col-span-2">
    <!-- 主线内容 -->
  </div>
  <div class="col-span-1">
    <div class="card p-3 bg-gray-50 text-xs">
      <div class="font-semibold mb-1">边注标题</div>
      <p>提示/警示/引用</p>
    </div>
  </div>
</div>
```

## 2. 视觉权重

**同一位置，不同样式。** 不改变内容位置，只改变显眼程度。

### 视觉权重的 5 个等级

| 等级 | 样式 | 适用 |
|---|---|---|
| L1 极突出 | `text-3xl+` + 状态色 + 加粗 + 卡片 | 北极星指标 / SLO 阈值 |
| L2 突出 | `text-lg` + 加粗 + 状态色边框 | 核心论点、关键约束 |
| L3 主线 | 常规文字 + 卡片 | 章节正文 |
| L4 次要 | `text-sm` + `text-ink-700` | 说明文字 |
| L5 边缘 | `text-xs` + `text-ink-500` | caption、元数据 |

```html
<!-- L1: 北极星 -->
<div class="text-5xl font-bold text-info-text display-num">≥ 25%</div>

<!-- L2: 核心论点 -->
<div class="text-base font-bold text-ink-900">关键约束</div>

<!-- L3: 主线 -->
<p class="text-sm">常规段落</p>

<!-- L4: 次要 -->
<p class="text-sm text-ink-700">说明</p>

<!-- L5: 边缘 -->
<p class="text-xs text-ink-500">caption</p>
```

**关键**：通过字号 + 颜色 + 字重 三个维度叠加控制权重，比单维度更立体。

## 3. Hover 悬浮（之前完全没覆盖的手段）

**不占主线空间，鼠标悬停时显示。** 阅读成本极低（不离开主线），但移动端不可用。

### 4 类 hover 实现

#### 类型 1：术语定义（`<abbr>` 原生）

适用：行业标准术语、缩写

```html
<abbr title="期望值 = 概率 × 价值 × 偏好匹配度"
      class="cursor-help border-b border-dashed border-info-text text-info-text">
  EV
</abbr>
```

**优点**：原生 HTML，无 JS，无障碍友好（屏幕阅读器会读 title）。
**缺点**：tooltip 样式无法自定义（浏览器默认）。

#### 类型 2：自定义 tooltip（Alpine.js）

适用：需要自定义样式的提示

```html
<span x-data="{ show: false }"
      @mouseenter="show = true"
      @mouseleave="show = false"
      class="relative cursor-help border-b border-dashed border-info-text text-info-text">
  术语
  <div x-show="show" x-cloak
       class="absolute bottom-full left-1/2 -translate-x-1/2 mb-1
              bg-gray-900 text-white text-xs px-2 py-1 rounded
              whitespace-nowrap z-50 pointer-events-none">
    详细定义（可含 HTML 格式）
    <div class="absolute top-full left-1/2 -translate-x-1/2
                border-4 border-transparent border-t-gray-900"></div>
  </div>
</span>
```

**优点**：样式完全自定义，可含富文本、链接、图片。
**缺点**：需要 Alpine.js。

#### 类型 3：纯 CSS hover

适用：无 JS 依赖场景

```html
<span class="tooltip-term relative cursor-help border-b border-dashed border-info-text">
  术语
  <span class="tooltip-content">
    定义
  </span>
</span>

<style>
.tooltip-term:hover .tooltip-content {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(0);
}
.tooltip-content {
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%) translateY(4px);
  background: #1f2328;
  color: white;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: all 0.15s;
  pointer-events: none;
  z-index: 50;
  margin-bottom: 4px;
}
.tooltip-content::after {
  content: '';
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 4px solid transparent;
  border-top-color: #1f2328;
}
</style>
```

#### 类型 4：图表节点 hover

适用：Mermaid/SVG 图表中节点的详细数据

```html
<!-- SVG 中给节点加 hover -->
<g class="cursor-pointer">
  <circle r="20" />
  <text>节点名</text>
  <title>hover 显示的详细数据（浏览器原生 tooltip）</title>
</g>
```

`<title>` 标签是 SVG 原生的 hover tooltip。

### Hover 的适用场景

| 场景 | 例子 | 推荐实现 |
|---|---|---|
| 术语定义 | "EV" "PII" "LTV" | `<abbr>` 或自定义 tooltip |
| 引用源摘要 | "按《X 文档》结论" hover 看摘要 | 自定义 tooltip |
| 错误码含义 | "Error 429" hover 看"限流" | `<abbr>` |
| 数据点精确值 | 图表节点 hover 看具体数字 | SVG `<title>` |
| 操作快捷键 | "复制"按钮 hover 看 `⌘C` | 原生 `title` 属性 |
| 缩略图全图 | 小图 hover 看大图 | CSS hover |

### Hover 的限制

- ❌ **移动端不可用**：手机没有 hover 状态。关键信息不能只藏在 hover 里。
- ❌ **键盘导航不可用**：Tab 键用户访问不到 hover 内容（除非用 `focus` 也触发）
- ❌ **长内容不适合**：用户必须保持鼠标位置，长内容体验差
- ❌ **截图/打印丢失**：hover 内容不会出现在截图或 PDF 里

**安全做法**：hover 只用于"补充信息"，主线必须能独立阅读。如果是必要信息，应该直接展开。

## 4. 折叠展开

详见 `folding-decision.md`。

三类折叠：
- **完全展开**：核心架构、决策依据
- **强化引导折叠**：重要但按需查阅（色条 + badge + 计数）
- **朴素折叠**：真次要（变更日志等）

## 5. 跳转链接

**关联但分散的内容用链接串联。**

### 同文档内锚点

```html
<!-- 引用 -->
<a href="#sec-5" class="text-info-text hover:underline">详见 5.1 节</a>

<!-- 章节锚点 -->
<section id="sec-5" class="section-anchor">
  <h2>5.1 子章节</h2>
</section>
```

### 跨文档引用

```html
<a href="../other-doc.html#sec-4-2" class="text-info-text hover:underline">
  参见《其他文档》4.2 节
</a>
```

### 引用 badge（视觉强化）

```html
<a href="#sec-5" class="inline-flex items-center gap-1 px-2 py-0.5
                       bg-info-bg text-info-text text-xs rounded
                       border border-info-border hover:bg-info-border/30">
  📚 详见 5.1
</a>
```

**适用场景**：
- 章节间相互引用（"参见 X.Y"）
- 术语链接到附录的术语表
- 引用其他文档

## 6. 附录归档

**几乎不看的内容放底部附录。**

```html
<section id="appendix">
  <h1>附录</h1>

  <details class="card mb-3 border-l-4 border-l-info-border">
    <summary>
      <span class="badge">📚 参考</span>
      <span class="text-[10px]">6 个引用</span>
      附录 A：与已有文档的引用关系
    </summary>
    <!-- 内容 -->
  </details>

  <details class="card mb-3 border-l-4 border-l-info-border">
    <summary>
      <span class="badge">📖 术语</span>
      <span class="text-[10px]">10 个</span>
      附录 B：术语表
    </summary>
    <!-- 内容 -->
  </details>

  <details class="card">
    <!-- 朴素折叠：真次要 -->
    <summary>附录 C：变更日志</summary>
  </details>
</section>
```

## 分层决策流程

按"用户何时需要"判断：

| 用户场景 | 推荐手段 |
|---|---|
| **始终需要看** | 空间位置（主区）+ 视觉权重（突出） |
| **主线阅读时偶尔需要** | Hover（不破坏主线） |
| **主线已表达，深入时需要** | 折叠（强化引导） |
| **关联章节的关联点** | 跳转链接 |
| **跨章节通用约束** | 侧栏固定卡片 |
| **偶尔查阅的参考** | 附录（强化引导） |
| **几乎不看** | 附录（朴素折叠） |

## Hover vs 折叠的边界

| 维度 | Hover | 折叠 |
|---|---|---|
| 信息长度 | 短（一句话/一段话） | 长（多段/列表/表格） |
| 触发方式 | 鼠标悬停 | 点击 |
| 阅读成本 | 极低（不离开主线） | 中（需要主动展开） |
| 适合场景 | 定义、提示、引用摘要 | 完整论述、详细配置 |
| 移动端 | ❌ 不友好 | ✅ 友好 |
| 截图/打印 | ❌ 丢失 | ✅ 可展开后截图 |

**判断口诀**：
- "用户读主线时，扫一眼这个词想知道定义？" → Hover
- "用户读完主线后，可能想深入看完整论述？" → 折叠

## 反模式

1. **关键约束只藏在 hover 里**：移动端用户看不到，搜索引擎抓不到
2. **整段长文用 hover**：用户必须保持鼠标位置，体验差
3. **该用 hover 的用了折叠**：术语定义折叠到附录，用户每次都要跳转
4. **6 种手段混用无章法**：术语用 hover、约束用侧栏、配置用折叠、参考用链接——这本是对的，但全局规则要一致（比如所有术语都用 hover，所有决策依据都用折叠强化引导）
5. **忽视移动端**：把核心提示只放在 hover 里，手机用户完全看不到
6. **依赖 hover 做关键交互**：如"hover 显示价格"，移动端无法成交

## 自检清单

每段内容，问自己：

- [ ] 用户何时需要这段信息？（始终/偶尔/几乎不）
- [ ] 信息多长？（短/中/长）
- [ ] 文档可能被移动端访问吗？（影响 hover 可用性）
- [ ] 我选的手段是否最合适？（不是默认折叠）
- [ ] 全局规则一致吗？（同类内容用同种手段）

## 实战案例

### 案例 1：术语定义

**错误**：折叠到附录，用户读到术语要跳转。

```html
<details>
  <summary>术语表</summary>
  <p>EV = 期望值</p>
</details>
```

**正确**：内联 hover 定义。

```html
<p>使用 <abbr title="期望值 = 概率 × 价值 × 偏好">EV</abbr> 排序推荐</p>
```

### 案例 2：决策依据

**错误**：用 hover 显示 4 个理由。

```html
<span class="has-tooltip">必做不能降级
  <div class="tooltip">4 个长理由...</div>
</span>
```

**问题**：用户必须保持鼠标位置才能读完。

**正确**：折叠 + 强化引导。

```html
<details class="card border-l-4 border-l-warning-border">
  <summary>
    为什么必做不能降级
    <span class="badge">🔒 决策依据</span>
    <span class="text-[10px]">4 个理由</span>
  </summary>
  <!-- 完整 4 个理由 -->
</details>
```

### 案例 3：跨章节约束

**错误**：每章都重复一次"6 条红线"。

**正确**：侧栏固定卡片 + 章节内引用。

```html
<!-- 侧栏（始终可见） -->
<aside>
  <div class="constraints-card">6 条红线...</div>
</aside>

<!-- 章节内 -->
<callout>注意 8.4 红线（详见侧栏）</callout>
```
