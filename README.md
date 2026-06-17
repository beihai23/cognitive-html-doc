# cognitive-html-doc

把密集、线性的 Markdown 技术/产品文档重构为**认知降维**的工业级单文件 HTML 文档。

读者阅读技术文档时分三种节奏，这个 skill 让产出同时满足：
- **3 秒抓核心**：Hero 区一句话定位 + 4 维度卡片
- **30 秒理解全貌**：主架构图（颜色编码）
- **3 分钟查细节**：固定侧边栏 TOC + 滚动高亮

## 何时使用

触发场景：

- 用户说"把这份 markdown 转成 HTML"
- 用户要"漂亮的 HTML 文档"、"工业级 HTML"
- 用户给一份 markdown 蓝图 + 提示词要求"高可读性、零依赖、单文件"
- 用户提到"降低认知负荷"或"扫视即可获取"
- 长文档（>500 行）需要 TOC、章节锚点、状态色系统

**不要用于**：
- 一行命令搞定的 markdown 渲染（`pandoc` 即可）
- 纯打印 PDF
- 不需要交互的简单文档

## 安装

### 方式 1：拷贝到全局 skills 目录

```bash
cp -r /path/to/cognitive-html-doc ~/.claude/skills/
```

### 方式 2：作为项目级 skill

```bash
cp -r /path/to/cognitive-html-doc .claude/skills/
```

### 方式 3：用 Claude Code plugin 系统

将整个目录打包为 `.skill` 文件后通过 plugin 安装。

## 文件结构

```
cognitive-html-doc/
├── SKILL.md                          # skill 核心（Claude 加载这个）
├── README.md                         # 本文件
├── references/                       # 详细指南（按需加载）
│   ├── layering-patterns.md          # 信息分层 6 类手段（空间/视觉/hover/折叠/跳转/附录）
│   ├── cdn-stack.md                  # CDN 栈配置（Tailwind/Mermaid/Highlight/Alpine）
│   ├── visualization-patterns.md     # 内容→图表类型映射 + Mermaid 模式库
│   ├── folding-decision.md           # 折叠合法性判定（分层手段 4 的细化）
│   └── completeness-check.md         # 逻辑完整性检查清单
├── assets/                           # 模板和组件
│   ├── template.html                 # HTML 骨架模板（可直接拷贝改造）
│   └── components.md                 # 常用组件示例代码
└── evals/                            # 测试用例
    └── evals.json                    # 3 个真实场景测试
```

## 核心设计原则

### 1. 认知降维三原则

3 秒抓核心 / 30 秒理解全貌 / 3 分钟查细节。

### 2. 信息分层（6 类手段，不只是折叠）

| 手段 | 适用 |
|---|---|
| **空间位置** | 跨章节复用的约束 → 侧栏固定卡片 |
| **视觉权重** | 突出关键 → 关键数字大字号 vs 普通文字 |
| **Hover 悬浮** | 短信息按需查看 → 术语定义、引用摘要（注意：移动端不可用） |
| **折叠展开** | 整段按需查看 → 决策溯源、详细配置 |
| **跳转链接** | 关联但分散 → "详见 X.Y"、跨文档引用 |
| **附录归档** | 几乎不看 → 变更日志 |

**关键**：分层不等于折叠。该用 hover 的别用折叠，该用侧栏的别重复 N 次。

详见 `references/layering-patterns.md`。

### 3. 逻辑完整性检查

不能以"技术细节"为由省略关键环节。典型容易遗漏：
- 产品文档：**数据来源**、决策依据
- 技术方案：**错误处理**、回滚、监控
- API 文档：**鉴权**、限流、错误码

详见 `references/completeness-check.md`。

### 4. 视觉锚点

- 关键数字脱离表格（大字号 + `tabular-nums`）
- 关键约束前置（callout + 大字号数字）
- 状态色 4 态全局一致（success/warning/danger/info）

### 5. 系统性章节扫描

写完后逐章问"核心论点是否突出"。**不要只盯用户指出的单点**——如果用户指出一处问题，主动扫描全文找同类。

## CDN 栈

- **Tailwind CSS**（CDN + 自定义 theme）：样式系统
- **Mermaid.js**（CDN）：流程/时序/状态/架构/timeline 图表
- **Highlight.js**（CDN）：代码语法高亮
- **Alpine.js**（CDN, defer）：轻交互（Tab、Tooltip）

详见 `references/cdn-stack.md`。

## 设计来源

这个 skill 源自一次真实的产品文档重构工作。工作中沉淀了 9 条核心经验：

1. 认知降维三原则（3 秒/30 秒/3 分钟）
2. **信息分层 6 类手段**（空间/视觉/hover/折叠/跳转/附录——曾把分层等同于折叠，被指出"hover 算不算"）
3. 视觉锚点设计
4. CDN 栈选择
5. 图表转换指南
6. **逻辑完整性检查**（曾把"数据来源"误判为"技术细节"省略，导致产品逻辑断链）
7. **系统性章节扫描**（曾只盯用户指出的单点，被指出"其他章节也有类似问题"）
8. **多载体同步**（源 markdown 改动必须同步衍生 HTML）
9. **移动端可达性**（hover 在移动端不可用，关键信息不能只藏在 hover 里）

每条经验都有对应的反例和教训，详见 SKILL.md 的"常见反模式"部分。

## License

MIT
