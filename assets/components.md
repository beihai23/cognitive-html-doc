# 常用组件示例

## Hero 区

```html
<section id="hero" class="mb-16">
  <div class="text-xs uppercase tracking-[0.15em] text-ink-500 mb-3 font-semibold">
    产品设计文档 · Phase 1
  </div>
  <h1 class="text-4xl font-semibold mb-3 tracking-tight leading-tight">
    产品名 / 文档主标题
  </h1>
  <p class="text-xl text-ink-700 mb-6 leading-relaxed">
    一句话定位（带关键数字 <span class="font-semibold text-ink-900">$200</span>）
  </p>

  <div class="flex flex-wrap gap-3 text-xs text-ink-500 mb-8">
    <span class="px-2 py-1 bg-white border border-line rounded">日期</span>
    <span class="px-2 py-1 bg-white border border-line rounded">v1.0</span>
    <span class="badge bg-success-bg text-success border border-success-border">状态</span>
  </div>

  <!-- 4 维度卡片 -->
  <div class="grid grid-cols-4 gap-3 mb-8">
    <div class="card card-hover p-4">
      <div class="text-[11px] uppercase tracking-wider text-ink-500 font-semibold mb-1">维度</div>
      <div class="text-sm font-semibold text-ink-900 leading-snug">值</div>
      <div class="text-xs text-ink-500 mt-2">说明</div>
    </div>
    <!-- ×4 -->
  </div>

  <!-- 主架构图 -->
  <div class="card p-6">
    <div class="mermaid">graph TB ...</div>
  </div>
</section>
```

## 关键数字卡片

适用：北极星指标、规模、占比、目标值

```html
<div class="card border-2 border-info-border p-6 text-center">
  <div class="text-xs uppercase tracking-wider text-info-text font-semibold">对外指标</div>
  <div class="text-5xl font-bold text-info-text display-num my-3">≥ 25%</div>
  <div class="text-base font-semibold">W4 留存率</div>
  <div class="text-xs text-ink-500 mt-1">用户用得起来</div>
</div>
```

**关键**：
- `text-5xl` 大字号
- `display-num` 等宽数字（`tabular-nums`）
- `border-2` 加粗边框
- 状态色三态：`text-{color}` + `border-{color}-border` + 浅色背景

## 关键约束 关键数字卡片（前置突出）

适用：把"必须接受的现实约束"提到章节开头，配大字号数字

```html
<div class="card border-2 border-warning-border bg-warning-bg/40 p-4 mb-4">
  <div class="flex items-center gap-5">
    <div class="text-4xl">⚠️</div>
    <div class="flex-1">
      <div class="text-[11px] uppercase tracking-wider text-warning-text font-semibold mb-1">
        关键约束 · 必须接受
      </div>
      <div class="text-base font-bold text-ink-900">
        WhatsApp 渠道 <span class="text-warning-text">5/8 项能力受限</span>
      </div>
      <div class="text-xs text-ink-700 mt-1">说明文字</div>
    </div>
    <div class="text-center border-l border-warning-border pl-5">
      <div class="text-3xl font-bold text-warning-text display-num leading-none">5/8</div>
      <div class="text-[10px] text-ink-500 mt-1">能力受限</div>
    </div>
  </div>
</div>
```

## 状态色 Callout

4 种语义色，每种用于不同场景：

```html
<!-- info（蓝）：补充说明、引用 -->
<div class="callout bg-info-bg border-info-border p-3 rounded">
  <div class="font-semibold text-info-text text-sm mb-1">ℹ️ 标题</div>
  <p class="text-sm">内容</p>
</div>

<!-- success（绿）：核心论点、最佳实践 -->
<div class="callout bg-success-bg border-success-border p-3 rounded">
  <div class="font-semibold text-success-text text-sm mb-1">🎯 标题</div>
  <p class="text-sm">内容</p>
</div>

<!-- warning（黄）：注意事项、Phase 边界 -->
<div class="callout bg-warning-bg border-warning-border p-3 rounded">
  <div class="font-semibold text-warning-text text-sm mb-1">⚠️ 标题</div>
  <p class="text-sm">内容</p>
</div>

<!-- danger（红）：红线、必做约束 -->
<div class="callout bg-danger-bg border-danger-border p-3 rounded">
  <div class="font-semibold text-danger-text text-sm mb-1">🚨 标题</div>
  <p class="text-sm">内容</p>
</div>
```

## 强化 details（重要但按需查阅）

4 个强化元素：色条 + 图标颜色 + badge + 内容计数

```html
<details class="card border-l-4 border-l-warning-border">
  <summary class="px-4 py-3 cursor-pointer font-medium flex items-center hover:bg-gray-50">
    <span class="summary-icon-closed mr-2 text-warning-text">▸</span>
    <span class="summary-icon-open mr-2 text-warning-text">▾</span>
    <span class="text-sm flex-1">为什么必做不能降级</span>
    <span class="ml-auto flex items-center gap-2">
      <span class="badge bg-warning-bg text-warning text-[10px] border border-warning-border">🔒 决策依据</span>
      <span class="text-[10px] text-ink-500">4 个理由</span>
    </span>
  </summary>
  <div class="px-4 pb-4 text-sm space-y-2 border-t border-line-soft pt-3">
    <p>理由 1</p>
    <p>理由 2</p>
  </div>
</details>
```

**badge 性质分类**：
- `🔒 决策依据` — 黄色，决策溯源
- `📋 工程运营必读` — 黄色，落地细节
- `⚠️ 待决` — 黄色，未决项
- `📚 参考` — 蓝色，引用其他文档
- `📖 术语` — 蓝色，术语表

## 朴素 details（真次要）

只用于变更日志、归档等几乎不看的内容：

```html
<details class="card">
  <summary class="px-4 py-3 cursor-pointer text-sm font-medium flex items-center hover:bg-gray-50">
    <span class="summary-icon-closed mr-2 text-ink-500">▸</span>
    <span class="summary-icon-open mr-2 text-ink-500">▾</span>
    附录 C：变更日志
  </summary>
  <div class="px-4 pb-4 border-t border-line-soft pt-3">
    <!-- 内容 -->
  </div>
</details>
```

## 默认 open details（团队必看）

未决项、当前阶段关键约束：

```html
<details open class="card border-l-4 border-l-warning-border">
  <summary>...</summary>
  <div>...</div>
</details>
```

## 响应式表格

```html
<div class="overflow-x-auto border border-line rounded-lg mb-4">
  <table class="w-full text-sm data-table bg-white">
    <thead class="bg-gray-50">
      <tr>
        <th class="px-4 py-3 text-left font-semibold text-xs uppercase tracking-wider text-ink-700">列名</th>
      </tr>
    </thead>
    <tbody>
      <tr class="border-t border-line-soft">
        <td class="px-4 py-2.5 font-medium">值</td>
      </tr>
    </tbody>
  </table>
</div>
```

**关键 class**：
- `data-table`：自动斑马线 + hover 高亮（CSS 已定义）
- `overflow-x-auto`：长表格横向滚动
- `text-xs uppercase tracking-wider`：表头小写转大写
- `border-t border-line-soft`：行间分隔线

## 表格内状态色

```html
<td class="px-4 py-2.5 text-center text-success-text font-medium">✅ 可代做</td>
<td class="px-4 py-2.5 text-center text-warning-text font-medium">⚠️ 部分支持</td>
<td class="px-4 py-2.5 text-center text-danger-text font-medium">❌ 禁止</td>
```

适用：能力矩阵、风险矩阵、对比表。

## Tab 切换（Alpine.js）

适用：双人群/双方案对比（注意：如果对比是核心论点，不要用 Tab 隐藏，用并排卡片）

```html
<div x-data="{ tab: 'a' }">
  <div class="flex border-b border-line">
    <button @click="tab='a'"
            :class="tab==='a' ? 'border-info-border text-info-text bg-info-bg/30' : 'border-transparent text-ink-500'"
            class="px-5 py-2.5 text-sm border-b-2 -mb-px font-medium">
      Tab A
    </button>
    <button @click="tab='b'"
            :class="tab==='b' ? 'border-warning-border text-warning-text bg-warning-bg/30' : 'border-transparent text-ink-500'"
            class="px-5 py-2.5 text-sm border-b-2 -mb-px font-medium">
      Tab B
    </button>
  </div>

  <div x-show="tab==='a'" class="mt-5">
    <!-- A 内容 -->
  </div>
  <div x-show="tab==='b'" x-cloak class="mt-5">
    <!-- B 内容 -->
  </div>
</div>
```

## 并排对比卡片（推荐替代 Tab）

```html
<div class="grid grid-cols-2 gap-4 mb-6">
  <div class="card border-2 border-success-border bg-success-bg/20 p-5">
    <div class="flex items-center gap-3 mb-3">
      <div class="w-10 h-10 rounded-full bg-success-bg flex items-center justify-center text-xl">📱</div>
      <div class="flex-1">
        <div class="text-[10px] uppercase tracking-wider text-success-text font-semibold">渠道 A</div>
        <div class="font-bold text-success-text text-sm">名称</div>
      </div>
      <div class="text-right">
        <div class="text-2xl font-bold text-success-text display-num leading-none">数字</div>
        <div class="text-[10px] text-ink-500 mt-0.5">说明</div>
      </div>
    </div>
    <!-- 详情 -->
  </div>

  <div class="card border-2 border-warning-border bg-warning-bg/20 p-5">
    <!-- 类似结构，warning 色 -->
  </div>
</div>
```

## 渐变背景卡片（互补可视化）

```html
<div class="card p-4 mb-5"
     style="background: linear-gradient(90deg, rgba(232,245,238,0.4) 0%, rgba(221,244,255,0.4) 50%, rgba(255,248,197,0.4) 100%);">
  <div class="flex items-center gap-6">
    <div class="flex-1">
      <div class="text-[10px] uppercase tracking-wider text-info-text font-semibold mb-1">
        🎯 核心论点
      </div>
      <div class="text-sm font-semibold text-ink-900 mb-1">论点</div>
      <div class="text-xs text-ink-700">解释</div>
    </div>
    <svg viewBox="0 0 200 110" class="w-36 flex-shrink-0">
      <!-- Venn 图 -->
    </svg>
  </div>
</div>
```

## 章节锚点 + TOC 链接

```html
<!-- TOC -->
<a href="#sec-1" class="toc-link block px-3 py-1.5 rounded text-ink-700 hover:bg-gray-200 border-l-2 border-transparent font-semibold">
  1. 章节名
</a>

<!-- 章节锚点 -->
<section id="sec-1" class="mb-16 section-anchor">
  <h1 class="text-2xl mb-2">1. 章节名</h1>
</section>
```

`section-anchor` class 提供 `scroll-margin-top: 80px`（避开 sticky nav）。

## 三步流程卡片

```html
<div class="grid grid-cols-3 gap-3 mb-4">
  <div class="card border-info-border bg-info-bg/20 p-4 text-center">
    <div class="text-3xl font-bold text-info-text display-num">7d</div>
    <div class="text-xs text-ink-700 mt-1">提前提醒</div>
  </div>
  <div class="card border-warning-border bg-warning-bg/20 p-4 text-center">
    <div class="text-3xl font-bold text-warning-text display-num">24h</div>
    <div class="text-xs text-ink-700 mt-1">关键钩子</div>
  </div>
  <div class="card border-danger-border bg-danger-bg/20 p-4 text-center">
    <div class="text-3xl font-bold text-danger-text display-num">当天</div>
    <div class="text-xs text-ink-700 mt-1">最后机会</div>
  </div>
</div>
```

适用：时间节点（到期提醒、阶段演进）、递进关系（升序严重度）。
