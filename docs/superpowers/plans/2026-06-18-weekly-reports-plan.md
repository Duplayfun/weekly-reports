# Weekly Reports 项目实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 搭建 weekly-reports 项目骨架，包括共享样式、两份周报的 HTML 模板、归档索引、GitHub Pages 部署配置，并生成第一期示范周报。

**Architecture:** 纯静态 HTML/CSS 项目，无外部依赖。`templates/base.css` 提供共享样式，两份周报各自有独立的 HTML 模板文件（含占位标记，每周由 Claude 填充数据后另存为新文件）。GitHub Actions 在 push 时自动部署到 GitHub Pages。

**Tech Stack:** HTML5 + CSS3（纯静态，无 JS 框架，无外部字体/图标库）

---

## 文件清单

| 文件 | 职责 |
|------|------|
| `.gitignore` | 忽略系统文件和 `.superpowers/` |
| `.github/workflows/deploy.yml` | GitHub Pages 自动部署 |
| `templates/base.css` | 两份周报共享的 Newsletter 样式 |
| `index.html` | 项目首页，链接到两份周报的最新版和归档 |
| `README.md` | 项目说明 |
| `patents/template.html` | 专利周报 HTML 模板（含占位数据） |
| `patents/archive/index.html` | 专利周报历史归档索引 |
| `amazon-de/template.html` | Amazon DE 周报 HTML 模板（含占位数据） |
| `amazon-de/archive/index.html` | Amazon DE 周报历史归档索引 |

---

### Task 1: 项目基础设施

**Files:**
- Create: `.gitignore`
- Create: 必要子目录

- [ ] **Step 1: 创建 .gitignore**

```bash
mkdir -p "d:/My Doc/website code/weekly-reports/patents/data" \
         "d:/My Doc/website code/weekly-reports/patents/reports" \
         "d:/My Doc/website code/weekly-reports/patents/archive" \
         "d:/My Doc/website code/weekly-reports/amazon-de/data" \
         "d:/My Doc/website code/weekly-reports/amazon-de/reports" \
         "d:/My Doc/website code/weekly-reports/amazon-de/archive" \
         "d:/My Doc/website code/weekly-reports/.github/workflows" \
         "d:/My Doc/website code/weekly-reports/templates"
```

- [ ] **Step 2: 写入 .gitignore**

Write file `d:\My Doc\website code\weekly-reports\.gitignore`:

```
.superpowers/
.DS_Store
Thumbs.db
```

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git init && git add .gitignore && git commit -m "chore: init project with .gitignore"
```

---

### Task 2: 共享样式 base.css

**Files:**
- Create: `templates/base.css`

- [ ] **Step 1: 写入共享样式文件**

Write file `d:\My Doc\website code\weekly-reports\templates\base.css`:

```css
/* ========================================
   Weekly Reports — 共享 Newsletter 样式
   无外部依赖，系统字体栈，响应式
   ======================================== */

/* --- 变量：两份周报各自覆盖 --- */
:root {
  --color-primary: #9a3412;
  --color-accent: #ea580c;
  --color-bg-light: #fff7ed;
  --color-card: #ffffff;
  --color-text: #1e293b;
  --color-text-secondary: #64748b;
  --color-border: #e2e8f0;
  --color-success: #16a34a;
  --color-danger: #dc2626;
  --color-neutral: #94a3b8;
  --max-width: 720px;
  --radius: 8px;
  --font-stack: -apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans SC", sans-serif;
}

/* --- 基础重置 --- */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--font-stack);
  color: var(--color-text);
  background: #f8fafc;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

/* --- 容器 --- */
.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 24px 16px;
}

/* --- 头部 --- */
.report-header {
  text-align: center;
  padding: 32px 16px 24px;
  border-bottom: 3px solid var(--color-accent);
  margin-bottom: 24px;
}

.report-header .week-label {
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--color-text-secondary);
  margin-bottom: 4px;
}

.report-header h1 {
  font-size: 24px;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: 4px;
}

.report-header .date-range {
  font-size: 13px;
  color: var(--color-text-secondary);
}

/* --- 摘要卡片 --- */
.summary-card {
  background: var(--color-bg-light);
  border-left: 4px solid var(--color-accent);
  padding: 16px 20px;
  border-radius: 0 var(--radius) var(--radius) 0;
  margin-bottom: 24px;
}

.summary-card .summary-title {
  font-size: 14px;
  font-weight: 700;
  color: var(--color-primary);
  margin-bottom: 8px;
}

.summary-card p {
  font-size: 13px;
  color: var(--color-text-secondary);
  line-height: 1.7;
}

.summary-card .tag {
  display: inline-block;
  background: #ffffff;
  border: 1px solid var(--color-border);
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 12px;
  margin: 2px 4px;
  white-space: nowrap;
}

/* --- 区块标题 --- */
.section-title {
  font-size: 16px;
  font-weight: 700;
  color: var(--color-text);
  margin: 28px 0 12px;
  padding-bottom: 8px;
  border-bottom: 2px solid var(--color-border);
}

/* --- 关键词标签 --- */
.keyword-cloud {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 24px;
}

.keyword-tag {
  background: #f1f5f9;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 13px;
  color: var(--color-text);
  border: 1px solid var(--color-border);
}

/* --- 数据表格 --- */
.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  margin-bottom: 24px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius);
  overflow: hidden;
}

.data-table thead th {
  background: #f8fafc;
  padding: 10px 12px;
  text-align: left;
  font-weight: 600;
  font-size: 12px;
  color: var(--color-text-secondary);
  border-bottom: 2px solid var(--color-border);
}

.data-table tbody td {
  padding: 10px 12px;
  border-bottom: 1px solid #f1f5f9;
  vertical-align: top;
}

.data-table tbody tr:hover { background: #fafbfc; }

.data-table .rank-col { width: 48px; text-align: center; font-weight: 700; }
.data-table .price-col { width: 80px; text-align: right; }
.data-table .score-col { width: 80px; text-align: center; }
.data-table .date-col { width: 90px; text-align: center; font-size: 12px; color: var(--color-text-secondary); }
.data-table .type-col { width: 80px; text-align: center; }

/* --- 评分星星 --- */
.stars { color: #f59e0b; letter-spacing: 2px; }
.stars-empty { color: #d1d5db; }

/* --- 趋势箭头 --- */
.trend-up { color: var(--color-success); font-weight: 700; }
.trend-down { color: var(--color-danger); font-weight: 700; }
.trend-flat { color: var(--color-neutral); }

/* --- 统计卡片行 --- */
.stats-row {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.stat-card {
  flex: 1;
  min-width: 120px;
  background: #f8fafc;
  padding: 16px;
  border-radius: var(--radius);
  text-align: center;
  border: 1px solid var(--color-border);
}

.stat-card .stat-number {
  font-size: 28px;
  font-weight: 700;
  color: var(--color-accent);
}

.stat-card .stat-label {
  font-size: 12px;
  color: var(--color-text-secondary);
  margin-top: 4px;
}

/* --- 精选条目卡片 --- */
.featured-list {
  margin-bottom: 24px;
}

.featured-item {
  border: 1px solid var(--color-border);
  border-radius: var(--radius);
  padding: 14px 16px;
  margin-bottom: 10px;
  background: #ffffff;
}

.featured-item .patent-title {
  font-size: 14px;
  font-weight: 600;
  color: var(--color-primary);
  margin-bottom: 4px;
}

.featured-item .patent-meta {
  font-size: 12px;
  color: var(--color-text-secondary);
  margin-bottom: 6px;
}

.featured-item .patent-analysis {
  font-size: 13px;
  color: var(--color-text);
  line-height: 1.6;
  margin-top: 6px;
  padding-top: 6px;
  border-top: 1px solid #f1f5f9;
}

.featured-item .patent-tags {
  display: flex;
  gap: 6px;
  align-items: center;
}

.badge {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 11px;
  font-weight: 600;
}

.badge-invention { background: #dcfce7; color: #166534; }
.badge-utility { background: #dbeafe; color: #1e40af; }
.badge-design { background: #fef3c7; color: #92400e; }

/* --- 重叠标记 --- */
.overlap-badge {
  display: inline-block;
  background: #fef3c7;
  color: #92400e;
  padding: 1px 6px;
  border-radius: 3px;
  font-size: 10px;
  font-weight: 600;
  margin-left: 4px;
}

/* --- 趋势小图 --- */
.trend-mini-chart {
  font-family: monospace;
  font-size: 11px;
  letter-spacing: 2px;
  color: var(--color-text-secondary);
}

/* --- 页脚 --- */
.report-footer {
  text-align: center;
  padding: 24px 16px;
  margin-top: 32px;
  border-top: 1px solid var(--color-border);
  font-size: 12px;
  color: var(--color-neutral);
}

.report-footer a {
  color: var(--color-accent);
  text-decoration: none;
}

.report-footer a:hover { text-decoration: underline; }

/* --- 响应式 --- */
@media (max-width: 600px) {
  .container { padding: 16px 12px; }
  .report-header h1 { font-size: 20px; }
  .stats-row { flex-direction: column; }
  .data-table { font-size: 12px; }
  .data-table thead th,
  .data-table tbody td { padding: 8px 6px; }
}
```

- [ ] **Step 2: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add templates/base.css && git commit -m "feat: add shared base CSS styles"
```

---

### Task 3: 专利周报 HTML 模板

**Files:**
- Create: `patents/template.html`

- [ ] **Step 1: 写入专利周报模板**

Write file `d:\My Doc\website code\weekly-reports\patents\template.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>专利周报 — 外贸玩具/产品相关专利分析 | [[WEEK_LABEL]]</title>
<link rel="stylesheet" href="../templates/base.css">
<style>
  :root {
    --color-primary: #9a3412;
    --color-accent: #ea580c;
    --color-bg-light: #fff7ed;
  }
</style>
</head>
<body>
<div class="container">

  <!-- ===== 头部 ===== -->
  <header class="report-header">
    <div class="week-label">专利周报 · 外贸玩具/产品</div>
    <h1>中国公开专利分析</h1>
    <div class="date-range">[[DATE_RANGE]] · 第 [[WEEK_NUM]] 周</div>
  </header>

  <!-- ===== 本周概要 ===== -->
  <div class="summary-card">
    <div class="summary-title">📊 本周概要</div>
    <p>
      [[SUMMARY_TEXT]]
    </p>
    <p style="margin-top:8px;">
      [[SUMMARY_TAGS]]
    </p>
  </div>

  <!-- ===== 热门关键词 ===== -->
  <h2 class="section-title">🔥 本周热门关键词</h2>
  <div class="keyword-cloud">
    [[KEYWORD_TAGS]]
  </div>

  <!-- ===== 精选专利 ===== -->
  <h2 class="section-title">📑 精选专利 — 高外贸潜力 Top 8</h2>
  <div class="featured-list">
    [[FEATURED_PATENTS]]
  </div>

  <!-- ===== 统计卡片 ===== -->
  <h2 class="section-title">📈 本周统计</h2>
  <div class="stats-row">
    <div class="stat-card">
      <div class="stat-number">[[TOTAL_COUNT]]</div>
      <div class="stat-label">本周相关专利</div>
    </div>
    <div class="stat-card">
      <div class="stat-number">[[HIGH_POTENTIAL_COUNT]]</div>
      <div class="stat-label">高外贸潜力（≥4★）</div>
    </div>
    <div class="stat-card">
      <div class="stat-number">[[KEY_APPLICANTS_COUNT]]</div>
      <div class="stat-label">重点申请人</div>
    </div>
  </div>

  <!-- ===== 完整列表 ===== -->
  <h2 class="section-title">📋 完整专利列表（[[TOTAL_COUNT]] 条）</h2>
  <table class="data-table">
    <thead>
      <tr>
        <th>专利名称</th>
        <th>申请人</th>
        <th class="type-col">类型</th>
        <th class="date-col">公开日</th>
        <th class="score-col">外贸潜力</th>
      </tr>
    </thead>
    <tbody>
      [[FULL_LIST_ROWS]]
    </tbody>
  </table>

  <!-- ===== 页脚 ===== -->
  <footer class="report-footer">
    <p>数据来源：CNIPA 中国国家知识产权局、SooPAT、智慧芽等公开数据库</p>
    <p>报告由 AI 辅助生成 · 生成时间：[[GENERATED_AT]]</p>
    <p><a href="archive/">📂 查看历史周报</a> · <a href="../index.html">🏠 项目首页</a></p>
  </footer>

</div>
</body>
</html>
```

- [ ] **Step 2: 验证模板可打开**

```bash
# 用浏览器打开确认结构正常
explorer "d:\My Doc\website code\weekly-reports\patents\template.html"
```

Expected: 浏览器显示模板骨架，CSS 正确加载，占位标记可见。

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add patents/template.html && git commit -m "feat: add patent report HTML template"
```

---

### Task 4: Amazon DE 周报 HTML 模板

**Files:**
- Create: `amazon-de/template.html`

- [ ] **Step 1: 写入 Amazon DE 周报模板**

Write file `d:\My Doc\website code\weekly-reports\amazon-de\template.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Amazon DE 周报 — 户外玩具/游戏 | [[WEEK_LABEL]]</title>
<link rel="stylesheet" href="../templates/base.css">
<style>
  :root {
    --color-primary: #92400e;
    --color-accent: #f59e0b;
    --color-bg-light: #fef3c7;
  }
</style>
</head>
<body>
<div class="container">

  <!-- ===== 头部 ===== -->
  <header class="report-header">
    <div class="week-label">Amazon DE 周报 · 户外玩具/游戏</div>
    <h1>新品 & 热卖排名趋势</h1>
    <div class="date-range">[[DATE_RANGE]] · 第 [[WEEK_NUM]] 周</div>
  </header>

  <!-- ===== 本周概览 ===== -->
  <div class="summary-card">
    <div class="summary-title">📊 本周概览</div>
    <p>[[SUMMARY_TEXT]]</p>
  </div>

  <!-- ===== 新品榜 ===== -->
  <h2 class="section-title">🆕 新品榜 Top 20</h2>
  <p style="font-size:12px;color:var(--color-text-secondary);margin-bottom:12px;">
    德国亚马逊 Outdoor Spielzeug 分类 · New Releases
  </p>
  <table class="data-table">
    <thead>
      <tr>
        <th class="rank-col">#</th>
        <th>产品名称</th>
        <th class="price-col">价格</th>
        <th class="score-col">评分</th>
        <th style="width:60px;text-align:center;">热卖</th>
      </tr>
    </thead>
    <tbody>
      [[NEW_RELEASES_ROWS]]
    </tbody>
  </table>

  <!-- ===== 热卖榜 ===== -->
  <h2 class="section-title">🔥 热卖榜 Top 20</h2>
  <p style="font-size:12px;color:var(--color-text-secondary);margin-bottom:12px;">
    德国亚马逊 Outdoor Spielzeug 分类 · Best Sellers
  </p>
  <table class="data-table">
    <thead>
      <tr>
        <th class="rank-col">#</th>
        <th>产品名称</th>
        <th class="price-col">价格</th>
        <th class="score-col">评分</th>
        <th>趋势</th>
      </tr>
    </thead>
    <tbody>
      [[BEST_SELLERS_ROWS]]
    </tbody>
  </table>

  <!-- ===== 排名变化追踪 ===== -->
  <h2 class="section-title">📈 排名变化追踪 — 热卖榜 Top 5</h2>
  <p style="font-size:12px;color:var(--color-text-secondary);margin-bottom:12px;">
    近四周排名走势
  </p>
  [[TRENDING_CHART]]

  <!-- ===== 统计卡片 ===== -->
  <h2 class="section-title">📊 本周统计</h2>
  <div class="stats-row">
    <div class="stat-card">
      <div class="stat-number">[[NEW_ON_BESTSELLERS]]</div>
      <div class="stat-label">新品同时上榜热卖</div>
    </div>
    <div class="stat-card">
      <div class="stat-number">[[AVG_NEW_PRICE]]</div>
      <div class="stat-label">新品均价</div>
    </div>
    <div class="stat-card">
      <div class="stat-number">[[AVG_BEST_PRICE]]</div>
      <div class="stat-label">热卖均价</div>
    </div>
  </div>

  <!-- ===== 页脚 ===== -->
  <footer class="report-footer">
    <p>数据来源：amazon.de · 分类：Outdoor Spielzeug / Outdoor Spiele</p>
    <p>报告由 AI 辅助生成 · 生成时间：[[GENERATED_AT]]</p>
    <p><a href="archive/">📂 查看历史周报</a> · <a href="../index.html">🏠 项目首页</a></p>
  </footer>

</div>
</body>
</html>
```

- [ ] **Step 2: 验证模板可打开**

```bash
explorer "d:\My Doc\website code\weekly-reports\amazon-de\template.html"
```

Expected: 浏览器显示模板骨架，CSS 正确加载，琥珀色主题。

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add amazon-de/template.html && git commit -m "feat: add Amazon DE report HTML template"
```

---

### Task 5: 项目首页 index.html

**Files:**
- Create: `index.html`

- [ ] **Step 1: 写入项目首页**

Write file `d:\My Doc\website code\weekly-reports\index.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Weekly Reports — 周报中心</title>
<style>
  :root {
    --font-stack: -apple-system, BlinkMacSystemFont, "Segoe UI", "Noto Sans SC", sans-serif;
  }
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: var(--font-stack);
    background: #f8fafc;
    color: #1e293b;
    line-height: 1.6;
  }
  .container { max-width: 640px; margin: 0 auto; padding: 48px 16px; }
  header { text-align: center; margin-bottom: 40px; }
  header h1 { font-size: 28px; color: #1e293b; }
  header p { color: #64748b; font-size: 14px; margin-top: 8px; }
  .reports { display: flex; flex-direction: column; gap: 20px; }
  .report-card {
    display: block;
    text-decoration: none;
    background: #ffffff;
    border: 1px solid #e2e8f0;
    border-radius: 12px;
    padding: 24px;
    transition: box-shadow 0.2s;
  }
  .report-card:hover { box-shadow: 0 4px 12px rgba(0,0,0,0.08); }
  .report-card .icon { font-size: 32px; margin-bottom: 8px; }
  .report-card h2 { font-size: 18px; margin-bottom: 4px; }
  .report-card.patent h2 { color: #9a3412; }
  .report-card.amazon h2 { color: #92400e; }
  .report-card p { font-size: 13px; color: #64748b; }
  .report-card .links { margin-top: 12px; display: flex; gap: 12px; flex-wrap: wrap; }
  .report-card .links a {
    display: inline-block;
    padding: 6px 14px;
    border-radius: 6px;
    font-size: 13px;
    text-decoration: none;
    font-weight: 500;
  }
  .btn-primary {
    background: #1e293b;
    color: #ffffff;
  }
  .btn-secondary {
    background: #f1f5f9;
    color: #475569;
  }
  footer { text-align: center; margin-top: 48px; font-size: 12px; color: #94a3b8; }
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>📊 Weekly Reports</h1>
    <p>每周自动生成 · 专利分析 & 亚马逊德国户外玩具趋势</p>
  </header>

  <div class="reports">
    <!-- 专利周报 -->
    <div class="report-card patent">
      <div class="icon">📋</div>
      <h2>专利周报</h2>
      <p>中国公开专利分析 — 外贸玩具及可外贸销售产品相关</p>
      <div class="links">
        <a href="patents/reports/" class="btn-primary">📖 查看最新一期</a>
        <a href="patents/archive/" class="btn-secondary">📂 历史归档</a>
      </div>
    </div>

    <!-- Amazon DE 周报 -->
    <div class="report-card amazon">
      <div class="icon">🛒</div>
      <h2>Amazon DE 周报</h2>
      <p>德国亚马逊户外玩具/游戏 · 新品榜 & 热卖榜排名趋势</p>
      <div class="links">
        <a href="amazon-de/reports/" class="btn-primary">📖 查看最新一期</a>
        <a href="amazon-de/archive/" class="btn-secondary">📂 历史归档</a>
      </div>
    </div>
  </div>

  <footer>
    <p>报告由 AI 辅助生成 · 每周五更新 · 数据来源：CNIPA / SooPAT / amazon.de</p>
  </footer>
</div>
</body>
</html>
```

- [ ] **Step 2: 验证**

```bash
explorer "d:\My Doc\website code\weekly-reports\index.html"
```

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add index.html && git commit -m "feat: add project index page"
```

---

### Task 6: 归档索引页

**Files:**
- Create: `patents/archive/index.html`
- Create: `amazon-de/archive/index.html`

- [ ] **Step 1: 写入专利归档索引**

Write file `d:\My Doc\website code\weekly-reports\patents\archive\index.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>专利周报 — 历史归档</title>
<link rel="stylesheet" href="../../templates/base.css">
<style>:root { --color-primary: #9a3412; --color-accent: #ea580c; --color-bg-light: #fff7ed; }</style>
</head>
<body>
<div class="container">
  <header class="report-header">
    <div class="week-label">专利周报 · 外贸玩具/产品</div>
    <h1>📂 历史归档</h1>
  </header>

  <table class="data-table">
    <thead>
      <tr>
        <th>周次</th>
        <th>日期范围</th>
        <th>专利数</th>
        <th>精选数</th>
        <th>链接</th>
      </tr>
    </thead>
    <tbody id="archive-list">
      <!-- 每周生成后在此添加新行 -->
      <!--
      <tr>
        <td><strong>W25</strong></td>
        <td>2026-06-15 ~ 2026-06-19</td>
        <td>42</td>
        <td>8</td>
        <td><a href="../reports/2026-W25.html">查看</a></td>
      </tr>
      -->
    </tbody>
  </table>

  <footer class="report-footer">
    <p><a href="../template.html">📋 查看模板</a> · <a href="../../index.html">🏠 项目首页</a></p>
  </footer>
</div>
</body>
</html>
```

- [ ] **Step 2: 写入 Amazon DE 归档索引**

Write file `d:\My Doc\website code\weekly-reports\amazon-de\archive\index.html`:

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Amazon DE 周报 — 历史归档</title>
<link rel="stylesheet" href="../../templates/base.css">
<style>:root { --color-primary: #92400e; --color-accent: #f59e0b; --color-bg-light: #fef3c7; }</style>
</head>
<body>
<div class="container">
  <header class="report-header">
    <div class="week-label">Amazon DE 周报 · 户外玩具/游戏</div>
    <h1>📂 历史归档</h1>
  </header>

  <table class="data-table">
    <thead>
      <tr>
        <th>周次</th>
        <th>日期范围</th>
        <th>新品数</th>
        <th>热卖数</th>
        <th>链接</th>
      </tr>
    </thead>
    <tbody id="archive-list">
      <!-- 每周生成后在此添加新行 -->
    </tbody>
  </table>

  <footer class="report-footer">
    <p><a href="../template.html">📋 查看模板</a> · <a href="../../index.html">🏠 项目首页</a></p>
  </footer>
</div>
</body>
</html>
```

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add patents/archive/index.html amazon-de/archive/index.html && git commit -m "feat: add archive index pages"
```

---

### Task 7: GitHub Actions 部署配置 + README

**Files:**
- Create: `.github/workflows/deploy.yml`
- Create: `README.md`

- [ ] **Step 1: 写入部署工作流**

Write file `d:\My Doc\website code\weekly-reports\.github\workflows\deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 2: 写入 README**

Write file `d:\My Doc\website code\weekly-reports\README.md`:

```markdown
# Weekly Reports

每周自动生成两份周报：

- **📋 专利周报** — 中国公开专利分析（外贸玩具/产品相关），数据来源 CNIPA / SooPAT
- **🛒 Amazon DE 周报** — 德国亚马逊户外玩具/游戏新品榜 & 热卖榜排名趋势

## 项目结构

- `patents/` — 专利周报（template.html → reports/ → archive/）
- `amazon-de/` — Amazon DE 周报（template.html → reports/ → archive/）
- `templates/base.css` — 共享样式
- `.github/workflows/deploy.yml` — GitHub Pages 自动部署

## 使用方式

每周五在对话中触发 AI 生成：

1. AI 采集最新数据（专利 + Amazon）
2. 数据写入 `data/`（JSON）
3. HTML 报告写入 `reports/`
4. 更新 `archive/index.html`
5. git push → GitHub Pages 自动发布

## 在线查看

部署后访问：`https://<username>.github.io/weekly-reports/`
```

- [ ] **Step 3: 提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add .github/workflows/deploy.yml README.md && git commit -m "feat: add GitHub Pages deploy config and README"
```

---

### Task 8: 最终验证

- [ ] **Step 1: 检查所有文件到位**

```bash
cd "d:/My Doc/website code/weekly-reports" && find . -type f ! -path './.git/*' ! -path './.superpowers/*' ! -path './docs/*' | sort
```

Expected output:
```
./.github/workflows/deploy.yml
./.gitignore
./README.md
./amazon-de/archive/index.html
./amazon-de/template.html
./index.html
./patents/archive/index.html
./patents/template.html
./templates/base.css
```

- [ ] **Step 2: 浏览器验证所有页面**

依次打开以下文件确认显示正常：
- `index.html` — 首页两个卡片，链接可点击
- `patents/template.html` — 专利模板，橙红色调，CSS 正确
- `amazon-de/template.html` — Amazon 模板，琥珀色调，CSS 正确
- `patents/archive/index.html` — 空归档页，结构正常
- `amazon-de/archive/index.html` — 空归档页，结构正常

- [ ] **Step 3: 最终提交**

```bash
cd "d:/My Doc/website code/weekly-reports" && git add -A && git status
```

确认无遗漏后：
```bash
git commit -m "chore: finalize project skeleton"
```

---

## 实现后事项

骨架搭建完成后，下一步是：

1. **创建 GitHub 仓库**并 push 代码 → 触发 Pages 部署
2. **生成第一期周报（今天，周三 W25 特别版）**：用户触发 → AI 采集数据 → 填充模板生成 `2026-W25.html`
3. **后续节奏**：固定每周六生成，用户触发；若周六没空可延后触发
4. **更新归档索引**：在 archive/index.html 中添加第一期链接
