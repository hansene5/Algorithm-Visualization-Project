# Algorithm-Visualization-Project UX Design Specification

_Created on 2025-11-19 by BMad_
_Generated using BMad Method - Create UX Design Workflow v1.0_

---

## Executive Summary

**AlgoVision** 是一个面向算法学习者的交互式可视化平台，通过实时的、可暂停的算法执行和深度监控，帮助学生深入理解数据结构和算法的内部工作原理。

**设计理念**: 这不是另一个"算法动画播放器"，而是一个让学生**看见思考过程**的学习伴侣。

**项目类型**:
- 技术类型: 单页Web应用 (SPA)
- 领域: 教育科技 (EdTech)
- 复杂度: 中低等
- 项目状态: Brownfield（基于现有6个模块扩展到30+）

**目标用户**:
- 计算机科学专业学生学习数据结构和算法
- 自学者准备技术面试（LeetCode练习）
- 教育工作者演示算法概念
- 任何需要真正*理解*算法工作原理的人

**设计继承**: 延续现有暗黑科技风 + Glassmorphism + Cyan/Purple渐变色系

---

## 1. Design System Foundation

### 1.1 Design System Choice

**决策**: 延续现有 Tailwind CSS + 自定义组件架构

**理由**:
- ✅ **轻量化原则**: PRD 明确要求零构建工具、纯浏览器运行
- ✅ **已有投资**: 现有设计已建立完整的 Tailwind 配置和自定义样式
- ✅ **CDN 依赖**: Tailwind CSS 通过 CDN 加载，无需本地构建
- ✅ **快速迭代**: 实用类优先（utility-first）适合快速原型和迭代

**设计系统组成**:

**1. Tailwind CSS v3 (CDN)**
- 核心样式框架
- 响应式工具类
- 暗黑模式支持
- 自定义配置（已扩展颜色、字体、动画）

**2. 自定义组件库**
- `.glass-panel` - 毛玻璃容器
- `.glass-card` - 悬停卡片
- `.bar` - 可视化柱状图
- `.node` - 图节点
- `.sq-item` - 栈/队列项
- 监控面板组件（变量显示、代码高亮）

**3. 图标系统**
- Lucide Icons (CDN)
- 一致的图标语言
- 24x24 标准尺寸

**提供的能力**:
- ✅ 完整的UI组件（按钮、表单、卡片、面板）
- ✅ 可访问性基础（WCAG AA 对比度）
- ✅ 响应式模式（Tailwind 断点）
- ✅ 暗黑主题（已配置）
- ✅ 动画系统（fade-in, slide-in）

**定制需求**:
- 🆕 **新增监控组件**: 调用栈可视化、内存布局、复杂度图表
- 🆕 **LeetCode 集成组件**: 题目卡片、难度徽章、题目面板
- 🆕 **可调整分栏**: Resize handle 组件
- 🆕 **引导组件**: 首次使用提示、空状态

---

## 2. Core User Experience

### 2.1 Defining Experience

**核心体验定义**: "像调试器一样单步执行算法，看每个变量怎么变化"

**体验特征**:
- **ONE核心动作**: 单步执行（Step）+ 实时监控变量状态
- **必须毫不费力**: 一键启动，空格步进，直观显示变量变化
- **最关键交互**: 暂停/步进控制 + 变量状态面板的实时同步

**平台需求**:
- 桌面优先（≥1024px 笔记本/台式机）
- 现代浏览器（Chrome 90+, Firefox 88+, Safari 14+, Edge 90+）
- MVP阶段暂不优化移动端（但应能基本运行）

**与竞品差异**:
- VisuAlgo / Algorithm Visualizer: 动画播放器（看动画）
- AlgoVision: 算法调试器（看思考过程）+ LeetCode题目结合

### 2.2 Desired Emotional Response

**目标情感**: 掌控感与清晰感 + 恍然大悟的惊喜

**掌控感与清晰感** 🎯
- "我终于看懂算法在做什么了！"
- 像使用调试器一样的专业工具感
- 每一步都清晰可见、可控制、可预测
- 信息透明，没有黑盒

**恍然大悟的惊喜** 💡
- "原来是这样！之前一直不懂的地方瞬间明白了"
- 学习的"啊哈时刻"（Aha moment）
- 突破性理解的满足感
- 从困惑到清晰的转变

**设计含义**:
- **基础界面**: 专业、工具化、信息密集（不花哨）
- **交互反馈**: 关键步骤时视觉强调（如变量变化高亮）
- **视觉节奏**: 平稳执行 + 关键时刻的视觉punctuation
- **成功状态**: 清晰的完成确认，强化"我理解了"的感觉

**避免**:
- ❌ 过度娱乐化（不是游戏）
- ❌ 过度简化（不低估学生智力）
- ❌ 过度动画（干扰思考）

### 2.3 Inspiration Analysis & Current Design Audit

**灵感来源: LeetCode 平台**

从 LeetCode 2025 最新 UX 研究中学到：
- ✅ **三栏式布局** - 题目 | 代码 | 测试，明确的信息分区
- ✅ **Run/Submit 分离** - 清晰的测试 vs 提交流程
- ✅ **可调整分栏** - 用户控制信息密度
- ⚠️ **平衡复杂性与友好性** - 用户反馈：技术先进 ≠ 良好体验
- ⚠️ **避免过度灵活** - 太多选项反而降低可用性

**现有 AlgoVision 设计审计**

**视觉基础** ✅ 已确立，保持延续
- **主题**: Dark Mode (#020617 深蓝黑背景)
- **Glassmorphism**: 毛玻璃效果（`backdrop-filter: blur(12px)`）
- **品牌色**:
  - Cyan (#06b6d4) - 主色、对比状态
  - Purple (#8b5cf6) - 次要强调
  - Green (#10b981) - 成功/已排序
  - Yellow (#fbbf24) - 活跃/当前
  - Red (#ef4444) - 错误/交换
- **字体**:
  - Inter - 界面文字（清晰、现代）
  - JetBrains Mono - 代码/数据（等宽、专业）

**当前布局架构** ✅ 专业工具感
```
┌─────────────────────────────────────────────┐
│ [Sidebar 256px]  [Header]  [Main Content]  │
│ ┌──────────┐     ┌────────────────────────┐ │
│ │ Logo     │     │ Breadcrumb   Search    │ │
│ │          │     └────────────────────────┘ │
│ │ Nav      │     ┌────────────────────────┐ │
│ │ - Dash   │     │                        │ │
│ │ - DS     │     │   Dashboard 或         │ │
│ │ - Algo   │     │   Visualizer          │ │
│ │          │     │                        │ │
│ └──────────┘     └────────────────────────┘ │
└─────────────────────────────────────────────┘

Visualizer 模式（三栏式）：
┌──────────┬───────────────────┬────────────┐
│ Vars     │ Canvas           │            │
│ Panel    │ (Visualization)  │            │
│ 288px    │                  │            │
│          ├──────────────────┤            │
│ Settings │ Code View 192px  │            │
└──────────┴──────────────────┴────────────┘
```

**现有优势** 🎯 值得保留
1. ✅ **清晰的信息架构** - 左栏控制、中央展示、底部代码
2. ✅ **专业调试感** - Variables 面板 = IDE debugger 体验
3. ✅ **glassmorphism** - 高级感、不抢视觉焦点
4. ✅ **色彩语义一致** - 学生可快速识别状态（绿=完成、黄=当前、红=比较）
5. ✅ **控制按钮标准化** - Play/Pause/Step 通用图标

**需要增强的部分** 📈 MVP 扩展重点
1. ⚠️ **监控深度不足** - 当前只显示变量，缺少调用栈、内存布局、复杂度统计
2. ⚠️ **LeetCode 集成缺失** - 模块卡片没有题目链接、题目描述未展示
3. ⚠️ **代码同步不够** - 代码行高亮存在但不够明显
4. ⚠️ **分栏不可调** - 固定宽度，无法适应不同需求（如需要更大 canvas）
5. ⚠️ **空状态提示弱** - 新用户不知道如何开始

### 2.5 Core Experience Principles

**定义体验**: "算法调试器" - 单步执行与实时监控

基于核心体验（调试器式单步执行）和目标情感（掌控感+惊喜），建立以下体验原则：

#### 1. 速度原则: **即时响应**
- Generator 执行延迟 < 50ms（用户感知即时）
- 步进操作无延迟
- 动画30-60 FPS流畅

**设计决策**:
- 使用原生 Generator（无额外框架）
- CSS transitions 不超过 200ms
- 避免复杂计算阻塞UI

#### 2. 引导原则: **渐进式揭示**
- 默认显示核心信息（变量、当前操作）
- 高级功能可折叠（调用栈、内存布局）
- 空状态提供明确指引

**设计决策**:
- Variables 面板默认展开
- 高级监控标签式展示（可按需查看）
- 首次使用显示快捷键提示

#### 3. 灵活性原则: **适度控制**
- 完全控制执行流程（播放/暂停/步进/重置）
- 可调整执行速度（1x/2x/4x）
- 可自定义输入数据

**设计决策**:
- 提供速度滑块而非预设档位
- 数据输入为可选功能（默认随机生成）
- 分栏宽度可调整（但非必需）

#### 4. 反馈原则: **关键时刻强调**
- 变量变化时短暂高亮（200ms）
- 比较/交换操作颜色编码
- 完成时清晰的总结统计

**设计决策**:
- 高亮采用渐变淡出动画
- 颜色保持一致：黄色=当前、红色=比较、绿色=完成
- 避免过度庆祝动画（保持专业工具感）

---

**UX复杂度评估**: 中等

**复杂度指标分析**:
- 用户角色: 单一（算法学习者）
- 主要旅程: 3个核心流程（浏览→选择→执行监控）
- 交互复杂度: 中等（调试器式交互 + 可视化控制）
- 平台: 单平台（桌面Web）
- 实时协作: 无
- 新颖交互: Generator-based 步进执行（已实现）

**引导模式**: UX_INTERMEDIATE（用户技能水平：intermediate）
- 平衡设计术语与清晰解释
- 提供设计决策的简要上下文
- 确认关键决策点
- 使用熟悉的开发者类比（调试器、IDE）

**项目综合理解**:

**项目**: AlgoVision - 算法可视化学习平台
**愿景**: 让学生像使用调试器一样"看见"算法的思考过程
**目标用户**: 计算机科学学生、面试准备者、算法学习者
**核心体验**: 单步执行 + 实时变量监控（调试器式体验）
**期望情感**: 掌控感 + 恍然大悟的惊喜
**平台**: 桌面Web（≥1024px），现代浏览器
**灵感**: LeetCode（题目集成）+ IDE调试器（监控体验）
**现有设计**: 暗黑科技风 + Glassmorphism + Cyan/Purple 渐变

---

## 3. Visual Foundation

### 3.1 现有视觉系统（延续与增强）

**设计决策**: 完全延续现有的暗黑科技风 + Glassmorphism 设计，确保新功能无缝融入

#### 色彩系统 🎨

**品牌色彩**（已确立，保持不变）

```css
/* 主色调 - 用于强调和品牌识别 */
--brand-cyan: #06b6d4;      /* 主色、对比状态、CTA按钮 */
--brand-purple: #8b5cf6;    /* 次要强调、渐变搭配 */

/* 状态色彩 - 算法可视化语义 */
--status-green: #10b981;    /* 成功、已排序、完成状态 */
--status-yellow: #fbbf24;   /* 活跃、当前元素、正在处理 */
--status-red: #ef4444;      /* 错误、比较、交换操作 */

/* 背景系统 - 深色模式分层 */
--bg-deepest: #020617;      /* 最深层背景（body） */
--bg-deep: #0f172a;         /* 深层背景（主容器） */
--bg-elevated: #1e293b;     /* 提升层背景（卡片、面板） */

/* 文字色彩 */
--text-primary: #f1f5f9;    /* 主要文字（标题、正文） */
--text-secondary: #94a3b8;  /* 次要文字（说明、辅助） */
--text-muted: #64748b;      /* 弱化文字（占位符、禁用） */
```

**色彩语义映射**（算法可视化场景）

| 颜色 | 算法状态 | 示例场景 |
|------|---------|----------|
| 🟡 Yellow | 当前正在处理 | 排序中的比较元素、遍历中的当前节点 |
| 🔴 Red | 比较/交换操作 | 快排的 pivot 比较、交换动画 |
| 🟢 Green | 已完成/排序完成 | 排序后的元素、访问过的节点 |
| 🔵 Cyan | 辅助标记/路径 | BFS 路径标记、栈顶指针 |
| 🟣 Purple | 特殊状态 | 递归调用栈、子问题分割 |

**新增色彩需求**（扩展到新功能）

```css
/* LeetCode 难度徽章色彩 */
--leetcode-easy: #00b8a3;   /* 简单题目 */
--leetcode-medium: #ffc01e; /* 中等题目 */
--leetcode-hard: #ef4743;   /* 困难题目 */

/* 监控面板增强色彩 */
--monitor-info: #3b82f6;    /* 信息提示（复杂度、统计） */
--monitor-warning: #f59e0b; /* 警告（性能瓶颈） */
--monitor-success: #10b981; /* 成功（优化建议） */
```

#### 字体系统 📝

**已确立字体栈**

```css
/* 界面文字 - 清晰、现代、易读 */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
/* 用途: 标题、按钮、标签、说明文字 */

/* 代码/数据 - 等宽、专业、对齐 */
font-family: 'JetBrains Mono', 'Fira Code', 'Consolas', monospace;
/* 用途: 代码展示、变量值、数据结构、伪代码 */
```

**字体层级**（Tailwind 类名）

| 层级 | 字号 | 用途 | Tailwind 类 |
|------|------|------|------------|
| H1 标题 | 30px | 页面主标题 | `text-3xl font-bold` |
| H2 标题 | 24px | 区块标题 | `text-2xl font-semibold` |
| H3 标题 | 20px | 组件标题 | `text-xl font-semibold` |
| Body 正文 | 16px | 正文、说明 | `text-base` |
| Small 辅助 | 14px | 辅助文字 | `text-sm` |
| Code 代码 | 14px | 代码展示 | `text-sm font-mono` |
| Label 标签 | 12px | 小标签、徽章 | `text-xs` |

#### Glassmorphism 效果 ✨

**核心玻璃效果**（已实现的自定义类）

```css
.glass-panel {
  background: rgba(15, 23, 42, 0.6);  /* 60% 透明度深蓝 */
  backdrop-filter: blur(12px);         /* 12px 高斯模糊 */
  border: 1px solid rgba(148, 163, 184, 0.1);  /* 微弱边框 */
}

.glass-card {
  background: rgba(30, 41, 59, 0.4);   /* 40% 透明度提升层 */
  backdrop-filter: blur(8px);
  border: 1px solid rgba(148, 163, 184, 0.15);
  transition: all 0.3s ease;
}

.glass-card:hover {
  background: rgba(30, 41, 59, 0.6);   /* 悬停时提升不透明度 */
  border-color: rgba(6, 182, 212, 0.3); /* Cyan 边框高亮 */
  transform: translateY(-2px);          /* 微妙上浮 */
}
```

**应用场景**

- `.glass-panel` → 侧边栏、变量面板、监控面板（大容器）
- `.glass-card` → 算法卡片、LeetCode 题目卡、统计卡片（可交互卡片）

#### 间距系统 📏

**Tailwind 默认间距**（保持不变）

- 基础单位: `4px` (Tailwind: `1 = 0.25rem`)
- 常用间距:
  - `p-4` (16px) - 卡片内边距
  - `p-6` (24px) - 面板内边距
  - `gap-4` (16px) - 元素间隙
  - `space-y-4` (16px) - 垂直间距

**布局固定尺寸**（现有布局保持）

- Sidebar 宽度: `256px` (`w-64`)
- Variables 面板宽度: `288px` (`w-72`)
- Code view 高度: `192px` (`h-48`)

**新增可调整尺寸**（扩展需求）

- 分栏可通过 resize handle 调整（保留最小宽度 200px）
- Canvas 区域自适应剩余空间

#### 阴影与深度 🌗

**分层阴影系统**

```css
/* Level 1 - 微弱提升 */
box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);  /* 小卡片 */

/* Level 2 - 明显提升 */
box-shadow: 0 4px 6px rgba(0, 0, 0, 0.4);  /* 悬浮面板 */

/* Level 3 - 高层浮动 */
box-shadow: 0 10px 15px rgba(0, 0, 0, 0.5); /* Modal、Dropdown */

/* Glow - 品牌强调 */
box-shadow: 0 0 20px rgba(6, 182, 212, 0.3); /* Cyan 发光效果 */
```

#### 动画系统 ⚡

**过渡时长**（遵循速度原则 < 200ms）

```css
/* 快速响应 - 即时反馈 */
transition: all 0.15s ease;  /* 按钮、开关、小元素 */

/* 标准过渡 - 平滑变化 */
transition: all 0.2s ease;   /* 卡片悬停、面板展开 */

/* 复杂动画 - 视觉强调 */
animation: highlight 0.3s ease-out; /* 变量变化高亮 */
animation: pulse 0.5s ease-in-out;  /* 完成状态脉冲 */
```

**关键动画定义**

```css
/* 变量变化高亮（淡出效果） */
@keyframes highlight {
  0% { background-color: rgba(251, 191, 36, 0.3); }   /* Yellow 30% */
  100% { background-color: transparent; }
}

/* 元素淡入（首次加载） */
@keyframes fade-in {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

#### 图标系统 🎭

**Lucide Icons**（已使用）

- 来源: Lucide Icons CDN
- 标准尺寸: `24x24px` (`w-6 h-6`)
- 颜色: 继承父元素文字颜色
- 用途: 导航图标、控制按钮、状态指示

**常用图标映射**

| 图标 | 功能 | Lucide 名称 |
|------|------|------------|
| ▶️ | 播放/开始 | `play` |
| ⏸️ | 暂停 | `pause` |
| ⏭️ | 单步执行 | `skip-forward` |
| 🔄 | 重置/重启 | `rotate-ccw` |
| ⚙️ | 设置 | `settings` |
| 📊 | 统计/监控 | `bar-chart-2` |
| 🔍 | 搜索 | `search` |
| 📝 | 代码 | `code` |

### 3.2 视觉一致性规则

#### 色彩使用原则

1. **品牌色限定使用**
   - Cyan: 主 CTA、当前激活状态、链接
   - Purple: 次要强调、渐变搭配（与 Cyan 配对）
   - 避免过度使用，保持视觉焦点

2. **状态色语义一致**
   - Yellow = 当前操作/活跃状态
   - Red = 比较/交换/错误
   - Green = 完成/成功
   - 全局统一，不在不同模块中改变含义

3. **背景分层清晰**
   - Body (#020617) → 最深层
   - Panel (#0f172a + glass) → 中层
   - Card (#1e293b + glass) → 提升层
   - 避免层级模糊

#### 字体使用原则

1. **功能明确**
   - Inter: 所有界面文字（包括按钮、标题、说明）
   - JetBrains Mono: 所有代码、数据、变量值
   - 绝不混用

2. **对比度保障**
   - 主要文字 (#f1f5f9) 在深色背景上 ≥ 7:1 对比度
   - 次要文字 (#94a3b8) ≥ 4.5:1 对比度（WCAG AA）
   - 代码文字保持高对比，确保可读性

#### 动画使用原则

1. **关键时刻强调**
   - 变量变化: 200ms 黄色高亮淡出
   - 元素交换: 300ms 位置过渡
   - 完成状态: 500ms 绿色脉冲

2. **避免过度**
   - 不在常规交互中使用复杂动画
   - 保持专业工具感，不要游戏化
   - 动画必须有明确的信息传达目的

### 3.3 扩展到新功能的应用

#### 监控面板（新增）

**变量面板增强**
- 背景: `.glass-panel` 保持一致
- 标签: `text-sm text-cyan-400` (品牌色强调)
- 数值: `font-mono text-base text-gray-200`
- 变化高亮: `highlight` 动画

**调用栈可视化**
- 卡片: `.glass-card` 堆叠样式
- 当前栈帧: Yellow 左边框（4px）
- 返回值: Cyan 小徽章

**内存布局图**
- 容器: `.glass-panel`
- 内存块: `.glass-card` 网格布局
- 指针: Cyan 箭头动画

#### LeetCode 集成（新增）

**题目卡片**
- 基础: `.glass-card:hover` 交互
- 难度徽章:
  - Easy: `bg-leetcode-easy/20 text-leetcode-easy`
  - Medium: `bg-leetcode-medium/20 text-leetcode-medium`
  - Hard: `bg-leetcode-hard/20 text-leetcode-hard`
- 题目编号: `font-mono text-cyan-400`

**题目描述面板**
- 背景: `.glass-panel`
- 标题: `text-xl font-semibold text-gray-100`
- 正文: `text-base text-gray-300`
- 示例代码: `font-mono bg-dark-900/60`

#### 可调整分栏（新增）

**Resize Handle**
- 视觉: 3px 宽度垂直分隔线
- 默认: `bg-gray-700/30`
- 悬停: `bg-cyan-500 cursor-col-resize`
- 拖动: `bg-cyan-400` + 半透明遮罩

**Interactive Visualizations:**

- Color Theme Explorer: [ux-color-themes.html](./ux-color-themes.html)
- Interactive Color System Showcase (生成中...)

---

## 4. Design Direction

### 4.1 Chosen Design Approach

**设计方向**: 延续并增强 - 暗黑科技风 + Glassmorphism + 专业调试感

**决策理由**:

1. **已有投资保护**
   - ✅ 现有 6 个模块已建立清晰的视觉语言
   - ✅ 用户已熟悉当前设计风格和交互模式
   - ✅ 无需推翻重建，降低开发成本和风险

2. **用户明确要求**
   - 用户明确指示："基于整体目前存在页面的设计风格"
   - 要求延伸而非重新设计
   - 保持设计连续性和一致性

3. **设计优势验证**
   - 现有设计审计显示：清晰的信息架构、专业调试感、色彩语义一致
   - Glassmorphism 提供高级感且不抢视觉焦点
   - 暗黑主题适合长时间学习场景（减少眼疲劳）

**设计哲学**:

> "不是另一个算法动画播放器,而是让学生看见思考过程的专业调试工具"

**视觉基因** (Design DNA):

```
暗黑科技风
├─ 深蓝黑背景 (#020617)
├─ Cyan/Purple 品牌渐变
└─ 最小化干扰，专注内容

Glassmorphism
├─ 半透明毛玻璃 (backdrop-filter: blur)
├─ 微妙边框和阴影
└─ 分层清晰的信息架构

专业调试感
├─ IDE-like 变量监控面板
├─ 等宽字体展示数据
└─ 明确的状态色彩语义 (黄=当前、红=比较、绿=完成)
```

**设计增强重点** (基于 MVP 需求):

#### 1. 深度监控能力 📊

**当前**: 仅显示变量值
**增强**: 调用栈、内存布局、复杂度实时统计

**设计扩展**:
- 调用栈: `.glass-card` 堆叠卡片，当前栈帧左侧 Yellow 边框
- 内存布局: `.glass-panel` 容器 + 网格布局内存块
- 复杂度图表: 动态柱状图/折线图，Monitor 色彩系统

#### 2. LeetCode 集成 🏆

**当前**: 无题目链接和描述
**增强**: 题目卡片、难度徽章、题解入口

**设计扩展**:
- 算法卡片新增难度徽章（Easy/Medium/Hard 色彩系统）
- 题目编号用 `font-mono` + Cyan 色强调
- 题目描述面板可展开/折叠（渐进式揭示原则）

#### 3. 可调整布局 ↔️

**当前**: 固定宽度分栏
**增强**: Resize handles 可调整面板宽度

**设计扩展**:
- 3px 宽度垂直分隔线，悬停时 Cyan 高亮
- 保留最小宽度约束（200px）防止布局崩溃
- 记住用户调整偏好（localStorage）

#### 4. 空状态与引导 💡

**当前**: 引导提示较弱
**增强**: 首次使用提示、快捷键说明、空状态设计

**设计扩展**:
- 首次访问显示 Overlay 教程（可跳过）
- 空状态使用友好插图 + 明确的 CTA 按钮
- 快捷键提示浮层（按 `?` 键显示）

### 4.2 关键界面展示

**完整设计方向展示**: [ux-design-directions.html](./ux-design-directions.html)

#### 主要界面布局

**Dashboard 视图** (模块浏览)
```
┌────────────────────────────────────────────────────────┐
│ [Sidebar] │ [Header: AlgoVision - Dashboard]          │
│           │ ┌───────────┬───────────┬───────────┐     │
│  Logo     │ │ [Card]    │ [Card]    │ [Card]    │     │
│           │ │ Bubble    │ Quick     │ BFS       │     │
│  Nav:     │ │ Sort      │ Sort      │ Traversal │     │
│  • Dash   │ │ Easy 🟢   │ Medium🟡  │ Medium🟡  │     │
│  • DS     │ │ #LC 剑45  │ #LC 剑51  │ #LC 102   │     │
│  • Algo   │ └───────────┴───────────┴───────────┘     │
│           │ [Grid continues with more cards...]        │
└────────────────────────────────────────────────────────┘
```

**Visualizer 视图** (算法执行 - 核心视图)
```
┌─────────────────────────────────────────────────────────────┐
│ [Sidebar] │ [Header: Quick Sort - LeetCode #912]          │
├───────────┼─────────────────────────────────────────────────┤
│  Logo     │ ┌─[Variables]──┬─[Canvas]────────────┬[题目]─┐│
│           │ │ Variables:   │                     │LeetCode││
│  Nav:     │ │  i: 3 🟡    │   [Visualization]   │#912    ││
│  • Dash   │ │  pivot: 42  │   [Bar Chart]       │Medium  ││
│  • DS     │ │              │   [Animated]        │        ││
│  • Algo   │ │ Call Stack:  │                     │[描述]  ││
│           │ │  quickSort() │                     │给定数组││
│  [Search] │ │   ↳ left:0   │                     │...     ││
│           │ │              │                     │        ││
│           │ ├──────────────┴─────────────────────┴────────┤│
│           │ │ [Code View]                                 ││
│           │ │ function quickSort(arr, left, right) {      ││
│           │ │ > if (left < right) {  ← 当前行            ││
│           │ └─────────────────────────────────────────────┘│
│           │ [Controls: ▶️ Play | ⏸️ Pause | ⏭️ Step | 🔄] │
└─────────────────────────────────────────────────────────────┘
```

**增强监控视图** (新增 - 深度监控模式)
```
┌─[Variables]──┐
│ Variables:   │  ← 默认展开
│  i: 3 🟡     │
│  pivot: 42   │
│              │
│ [Tab] 调用栈  │  ← 可选标签页
│ [Tab] 内存    │
│ [Tab] 复杂度  │  ← 点击展开
├──────────────┤
│ 复杂度分析:   │
│ ┌─────────┐  │
│ │ 时间 █░░│  │  75% - O(n²)
│ │ 空间 █░░│  │  20% - O(log n)
│ └─────────┘  │
│ 操作统计:     │
│ • 比较: 142   │
│ • 交换: 38    │
└──────────────┘
```

### 4.3 交互设计原则

#### 响应式交互 (< 200ms)

```css
/* 按钮按下 */
button:active {
  transform: scale(0.95);
  transition: transform 0.1s ease;
}

/* 卡片悬停 */
.glass-card:hover {
  transform: translateY(-2px);
  transition: all 0.15s ease;
}

/* 变量变化高亮 */
.variable-changed {
  animation: highlight 0.3s ease-out;
}
```

#### 状态反馈清晰

- **执行中**: Canvas 元素动画 + 代码行高亮 (Yellow background)
- **暂停时**: 控制按钮状态切换 (Cyan 边框高亮)
- **完成后**: 成功提示 (Green 脉冲) + 统计面板显示

#### 信息渐进揭示

- **第一层** (默认): 变量值 + Canvas 可视化 + 代码
- **第二层** (可选): 调用栈标签页、内存布局
- **第三层** (高级): 复杂度图表、操作统计

### 4.4 可访问性设计

#### 色彩对比度

- 主要文字: ≥ 7:1 (超越 WCAG AAA)
- 次要文字: ≥ 4.5:1 (符合 WCAG AA)
- 交互元素: ≥ 3:1 边界对比

#### 键盘导航

```
空格键  → 单步执行 (Step)
Enter   → 播放/暂停切换
R       → 重置 (Reset)
?       → 显示快捷键帮助
Esc     → 关闭浮层/返回
Tab     → 焦点导航
```

#### 屏幕阅读器

- 所有交互元素包含 `aria-label`
- 动态内容变化使用 `aria-live="polite"`
- 代码高亮当前行宣告 "Current line: [line number]"

**Interactive Mockups:**

- Design Direction Showcase: [ux-design-directions.html](./ux-design-directions.html)

---

## 5. User Journey Flows

### 5.1 Critical User Paths

{{user_journey_flows}}

---

## 6. Component Library

### 6.1 Component Strategy

{{component_library_strategy}}

---

## 7. UX Pattern Decisions

### 7.1 Consistency Rules

{{ux_pattern_decisions}}

---

## 8. Responsive Design & Accessibility

### 8.1 Responsive Strategy

{{responsive_accessibility_strategy}}

---

## 9. Implementation Guidance

### 9.1 Completion Summary

{{completion_summary}}

---

## Appendix

### Related Documents

- Product Requirements: `docs/prd.md`
- Product Brief: `N/A`
- Brainstorming: `N/A`

### Core Interactive Deliverables

This UX Design Specification was created through visual collaboration:

- **Color Theme Visualizer**: /Users/xuyi/workDir/ideaProject/2_learning/Algorithm-Visualization-Project/docs/ux-color-themes.html
  - Interactive HTML showing all color theme options explored
  - Live UI component examples in each theme
  - Side-by-side comparison and semantic color usage

- **Design Direction Mockups**: /Users/xuyi/workDir/ideaProject/2_learning/Algorithm-Visualization-Project/docs/ux-design-directions.html
  - Interactive HTML with 6-8 complete design approaches
  - Full-screen mockups of key screens
  - Design philosophy and rationale for each direction

### Optional Enhancement Deliverables

_This section will be populated if additional UX artifacts are generated through follow-up workflows._

<!-- Additional deliverables added here by other workflows -->

### Next Steps & Follow-Up Workflows

This UX Design Specification can serve as input to:

- **Wireframe Generation Workflow** - Create detailed wireframes from user flows
- **Figma Design Workflow** - Generate Figma files via MCP integration
- **Interactive Prototype Workflow** - Build clickable HTML prototypes
- **Component Showcase Workflow** - Create interactive component library
- **AI Frontend Prompt Workflow** - Generate prompts for v0, Lovable, Bolt, etc.
- **Solution Architecture Workflow** - Define technical architecture with UX context

### Version History

| Date     | Version | Changes                         | Author        |
| -------- | ------- | ------------------------------- | ------------- |
| 2025-11-19 | 1.0     | Initial UX Design Specification | BMad |

---

_This UX Design Specification was created through collaborative design facilitation, not template generation. All decisions were made with user input and are documented with rationale._
