# CLAUDE.md — Lewis Lee 个人网站

> 单文件个人主页 · 纯 HTML/CSS/JS · 零外部依赖
> 设计参照 claude.com 暖色调 + 衬线/无衬线双字体系统

---

## 1. 项目概览

| 项目 | 说明 |
|------|------|
| **文件** | `index.html` (单文件, ~1617 行, ~50KB) |
| **技术栈** | 纯 HTML + 内联 CSS + 内联 JS，无框架/构建工具/外部 CDN |
| **语言** | 中文为主，英文为辅 |
| **结构** | 6 个 `<section>` 单页滚动 + 固定导航栏 + 右侧导航点 |
| **资源** | 1 张头像 (`头像.png`) + 75 张生活照片 (`生活照片/`) |
| **图标** | 6 个内联 SVG (Feather 风格, 20×20, 2px stroke) |

---

## 2. 设计令牌 (CSS Custom Properties)

### 2.1 颜色

```css
/* 背景 — 暖白三级 */
--bg-primary:    #FAF9F5;   /* 主背景 */
--bg-secondary:  #F5F4ED;   /* 次级背景 (经历/生活板块) */
--bg-tertiary:   #F0EEE6;   /* 第三级背景 (卡片) */

/* 文字 */
--text-primary:   #141413;   /* 主文字 */
--text-secondary: #30302E;   /* 次级文字 */
--text-tertiary:  #5E5D59;   /* 第三级文字 (标签/说明) */

/* 强调色 */
--accent:          #D97757;   /* 主品牌色 (Clay 橙棕) */
--accent-hover:    #C6613F;   /* 悬停态 */
--accent-secondary: #788C5D;  /* 橄榄绿 (第二强调 — 限用于标签背景/分类徽章) */

/* 边框 */
--border-primary:   #B0AEA5;  /* 主边框 */
--border-secondary: #D1CFC5;  /* 次级边框 */
--border-light:     #E8E6DC;  /* 浅边框/分隔线 */
```

### 2.2 字体

```css
--font-serif: "Noto Serif SC", "STSong", "Songti SC", "Georgia", "Times New Roman", serif;
--font-sans:  "PingFang SC", "Microsoft YaHei", "Hiragino Sans GB", -apple-system, "Segoe UI", sans-serif;
```

**规则**: 标题/展示文字用衬线 (`--font-serif`)，正文/UI/标签/按钮用无衬线 (`--font-sans`)。
`html` 默认字体为 `--font-sans`。

**字体分配**:
| 衬线 (`--font-serif`) | 无衬线 (`--font-sans`) |
|---|---|
| `.text-display`, `.text-headline-1~4` | `html` 基础 |
| `.about-name`, `.about-name-en` | `.about-tag`, `.about-cta` |
| `.section-title` | `.section-label` |
| `.experience-card-title`, `.experience-sub-title` | `.detail-label`, `.detail-value` |
| `.life-column-tag` | `.life-column-subtag`, `.life-column-placeholder` |
| | `.contact-label`, `.contact-value` |
| | `.nav-dot-label`, `.hero-scroll-text` |
| | `.hero-sub`, `.hero-cta`, `.hobby-tag` |

### 2.3 字号 (响应式 clamp)

| 令牌 | 值 | 用途 |
|------|-----|------|
| `--fs-display` | `clamp(42px, 6vw, 80px)` | Hero 座右铭 |
| `--fs-headline-1` | `clamp(32px, 4.5vw, 52px)` | 板块标题/姓名 |
| `--fs-headline-2` | `clamp(28px, 3.8vw, 44px)` | 二级标题 |
| `--fs-headline-3` | `clamp(24px, 3vw, 36px)` | 三级标题 |
| `--fs-headline-4` | `clamp(20px, 2.5vw, 32px)` | 英文副标题 |
| `--fs-body-1` | `clamp(17px, 2vw, 20px)` | 正文大 |
| `--fs-body-2` | `16px` | 正文/按钮 |
| `--fs-body-3` | `14px` | 标签/说明 |
| `--fs-caption` | `12px` | 小字标注 |

### 2.4 间距 (4px 网格)

`--sp-8` `--sp-12` `--sp-16` `--sp-20` `--sp-24` `--sp-32` `--sp-40` `--sp-48` `--sp-56` `--sp-64` `--sp-80` `--sp-96` `--sp-128` `--sp-200`

### 2.5 圆角

`--br-4` `--br-8` `--br-12` `--br-16` `--br-24` `--br-48` `--br-full` (50%)

- 按钮/标签/方框: `--br-8`
- 卡片: `--br-24`
- 头像/圆形元素: `--br-full`

### 2.6 动画

```css
--ease-out-quart:     cubic-bezier(0.165, 0.84, 0.44, 1);   /* 最常用 */
--ease-out-expo:      cubic-bezier(0.16, 1, 0.3, 1);
--ease-in-out-quart:  cubic-bezier(0.77, 0, 0.175, 1);
--transition-fast: 200ms var(--ease-out-quart);
--transition-base: 400ms var(--ease-out-quart);
--transition-slow: 750ms var(--ease-out-quart);
```

---

## 3. 页面结构

### 3.1 HTML 骨架

```
<body>
  <nav class="top-nav">         <!-- 固定顶部导航 -->
  <nav class="nav-dots">        <!-- 固定右侧导航点 -->
  <section id="hero">           <!-- 1. 首页 (座右铭 + 身份副标题 + CTA) -->
  <section id="about">          <!-- 2. 个人信息 -->
  <section id="experience">     <!-- 3. 经历 -->
  <section id="articles">       <!-- 4. 文章 (已隐藏，内容就绪后恢复) -->
  <section id="life">           <!-- 5. 生活 (三列滚动) -->
  <section id="contact">        <!-- 6. 联系方式 -->
  <div class="lightbox">        <!-- 灯箱叠加层 -->
  <script>                      <!-- 所有 JS -->
```

### 3.2 各板块详解

**Hero** (`#hero`)
- 座右铭: "非凡的主张需要非凡的证据"
- 浮动光晕: 2 个绝对定位的模糊椭圆 (`.hero-blob`)，颜色取自 `--accent` 和 `--accent-secondary`，12s~15s 慢速漂浮
- 逐字解码动画: JS Scramble 效果 — 文字先显示随机字符（字母/数字/符号），18 帧内逐步"解码"为正确内容。每字 0.04s 延迟启动，上限 1.2s
- `.hero-sub`: 身份副标题 (如"某某大学 · 专业方向")，1.4s 延迟淡入，`--text-tertiary`
- `.hero-cta`: 胶囊按钮 ("了解我 →")，1.8s 延迟淡入，hover 填充 `--accent`
- 底部 Scroll 指示器 (脉冲动画垂直线)

**About** (`#about`)
- 双列布局: 左侧头像 + 右侧信息
- 头像: `br-full` 圆形 + 静态渐变边框 (`accent → accent-secondary` 135deg，`linear-gradient` + `padding-box`/`border-box`)
- 内容: 姓名 → 英文名 → 身份标签 → "联系我"按钮 → 详情网格 → 爱好标签
- 详情: 2×2 网格 (学校/故乡/生日/MBTI)
- 按钮链接到 `#contact`

**Experience** (`#experience`)
- 背景 `--bg-secondary`
- 双列卡片: "比赛"(含子卡片 创新创业/工科竞赛) + "个人开发项目"

**Articles** (`#articles`)
- 已通过 CSS `display: none` 隐藏，顶部导航和右侧导航点中的对应链接已注释
- 内容就绪后取消隐藏并恢复导航项

**Life** (`#life`)
- 全屏 `height: 100vh; overflow: hidden;`
- 三列独立滚动: 旅行篇 (35张) / 美食篇 (31张) / 纪念篇 (9张，建议增至 15 张以上)
- 各列 `data-speed`: 0.30 / 0.22 / 0.16 px/帧
- 照片 4:3 比例，杂志风格：暖棕调 hover 阴影 + 克制缩放 + 极淡边框
- 粘性列标题 (带渐变遮罩 + 24px 细线分隔)
- 滚动条默认隐藏，hover 列时淡入
- 自动滚动: 用户触碰暂停 3 秒，到底循环回顶部

**Contact** (`#contact`)
- 居中卡片 (`--bg-tertiary` + `border`)
- 6 行联系方式，每行有内联 SVG 图标
- 邮箱和 GitHub 为链接，hover 时变 `--accent` 色
- 含链接的 `.contact-item` hover 有浅背景 (`--bg-tertiary` + `--br-8`)

---

## 4. JavaScript 功能

全部包裹在 IIFE `(function() { ... })();` 中。

### 4.1 逐字解码动画 (Scramble)
- 选择器: `.hero-line`, `.top-nav-name`
- 将文字拆为独立 `<span class="hero-char">`
- 每个字符从随机字符集 (字母+数字+符号) 开始，18 帧内逐步"解码"为正确内容
- 每字通过 `setTimeout` 延迟启动: `Math.min(i * 40, 1200)ms`
- 使用 `requestAnimationFrame` 驱动每帧更新

### 4.2 导航高亮 (IntersectionObserver)
- 观察所有 `section[id]`
- 阈值 0.4, rootMargin `-10%`
- 同步高亮 `.nav-dot` 和 `.top-nav-link`

### 4.3 滚动揭示 (IntersectionObserver)
- 观察所有 `[data-reveal]` 元素
- 阈值 0.1, rootMargin `0px 0px -40px 0px`
- 添加 `.revealed` 类后停止观察 (一次性)
- CSS: opacity 0 + translateX(-20px) → opacity 1 + translateX(0)（水平滑入）
- 过渡: `transform 0.8s var(--ease-out-expo), opacity 0.8s var(--ease-out-expo)`
- `data-reveal-delay="1"~"7"` 对应 0.1s~0.7s 延迟
- CSS: `[data-reveal-delay="6"] { transition-delay: 0.6s; }` `[data-reveal-delay="7"] { transition-delay: 0.7s; }`

### 4.4 噪点纹理背景
- `body::before` 伪元素，`position: fixed`
- 内联 SVG `<feTurbulence>` 生成分形噪点 (baseFrequency=0.65)
- `opacity: 0.03; pointer-events: none; z-index: 9999`

### 4.5 Hero 浮动光晕
- 2 个绝对定位 `<div class="hero-blob">`，`filter: blur(80px)`
- 颜色: `--accent` (#D97757) + `--accent-secondary` (#788C5D)
- CSS `@keyframes blobFloat`: 12s~15s ease-in-out infinite alternate，位移+缩放

### 4.6 光标跟随光晕
- `<div id="cursorGlow">` 固定定位 200px 圆形
- `radial-gradient` 从暖橙到透明
- JS `mousemove` 事件更新 `transform: translate(x-100, y-100)`
- `transition: transform 0.15s linear` 产生滞后跟随效果

### 4.7 生活板块自动滚动 (requestAnimationFrame)
- IntersectionObserver 检测 `#life` 可见性 (阈值 0.3)
- 每帧递增各列 `scrollTop` 对应 `data-speed` 值 (0.30 / 0.22 / 0.16 px/帧)
- 用户滚轮/触摸 → 该列暂停 3 秒
- 到底 → `scrollTo({ top: 0, behavior: 'smooth' })` 循环

### 4.8 灯箱
- 点击 `.life-photo img` → 显示全屏叠加层
- 点击背景/关闭按钮/Escape → 关闭
- 关闭后 400ms 清除 src

---

## 5. 响应式断点

| 断点 | CSS | 行为 |
|------|-----|------|
| 默认 | (无媒体查询) | 移动端优先，单列布局 |
| 平板 | `@media (max-width: 1023px)` | 导航点缩小、生活板块折叠为单列正常滚动、双列变单列 |
| 移动端 | `@media (max-width: 767px)` | 导航名隐藏、联系卡片/项目堆叠、生活照片单列 |

关键响应式变化:
- 生活板块: ≥1024px 三列独立 100vh 滚动 → <1024px 单列正常文档流
- 关于网格: <768px 单列
- 经历网格: <1024px 单列
- 联系项目: <768px 标签在上值在下

---

## 6. 资源文件

```
Lewis-personal-website/
├── index.html           # 唯一代码文件
├── CLAUDE.md            # 本文件
├── 头像.png             # 头像 (635KB)
└── 生活照片/
    ├── travel-01.jpg ~ travel-35.jpg    # 35 张旅行照片
    ├── food-01.jpg ~ food-31.jpg        # 31 张美食照片
    └── memory-01.jpg ~ memory-09.jpg    # 9 张纪念照片
```

**命名规范**: 英文前缀 + 两位数字编号，文件名不含空格，避免 URL 编码问题。
映射: `旅游 (N).jpg` → `travel-NN.jpg` / `美食 (N).jpg` → `food-NN.jpg` / `纪念 (N).jpg` → `memory-NN.jpg`

---

## 7. 修改指南

### 替换照片
1. 将新 `.jpg` 放入 `生活照片/`
2. 命名遵循英文规范: `travel-NN.jpg` / `food-NN.jpg` / `memory-NN.jpg`（不含空格）
3. 更新 HTML 中对应列的 `<div class="life-photo">` 条目

### 修改文字内容
- Hero 座右铭: `<h1 class="hero-motto">` 内的 `<span class="hero-line">`
- Hero 身份副标题: `.hero-sub` 元素
- Hero CTA 按钮: `.hero-cta` 元素文字
- 个人信息: `#about` 区域内的姓名/英文名/详情/爱好
- 联系方式: `#contact` 区域内各 `.contact-value`
- 联系方式链接 hover: `--accent` 色，含链接的 `.contact-item` 有浅背景

### 修改颜色
- 全局色彩在 `:root` 中定义 (第 11-90 行)
- 修改 `--accent` 等变量即可全局生效

### 修改字体
- 仅改 `:root` 中的 `--font-serif` / `--font-sans` 即可全局生效
- 各元素的 serif/sans 分配见第 2.2 节表格

### 添加新板块
1. 新增 `<section id="xxx" class="section-xxx">`
2. 在顶部 `.top-nav-links` 和 `.nav-dots` 中添加对应 `<a>` 链接
3. CSS 参照现有 `.section-*` 模式: `padding-block: var(--sp-128);`
4. 标题使用 `.section-label` + `.section-title` + `data-reveal` 属性

---

## 8. 设计原则

1. **暖白基调**: 主背景 `#FAF9F5` 而非纯白
2. **衬线标题 + 无衬线正文**: 参照 claude.com 的"思考感"标题策略
3. **克制配色**: 3 级灰阶 + 2 个强调色足够
4. **触控友好**: 4px 间距网格，8px 圆角按钮
5. **滚动叙事**: `data-reveal` 交错动画引导阅读节奏
6. **零依赖**: 纯手写 HTML/CSS/JS，无构建工具，无外部库
7. **无暗色模式**: 仅亮色主题
8. **第二强调色收敛**: `--accent-secondary` 仅用于标签背景和分类徽章，禁止扩展用途
