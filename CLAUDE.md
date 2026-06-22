# Weekly Reports — AI 生成指导

## 核心原则：数据真实性第一

**这是本项目最重要的规则，适用于所有周报类型（专利、Amazon 美欧 及后续新增的任何周报）。**

### 数据核实要求

1. **每条数据必须来自可追溯的原始来源**，不得编造、推测、拼凑
2. **专利周报**：
   - 每条专利的专利号必须在 CNIPA/天眼查/PatentGuru 等数据库中查证属实
   - 专利名称和内容必须与专利号对应，不得张冠李戴
   - 无专利号的数据，即使从新闻中看到，也不得列入完整列表（可在备注中提及）
3. **Amazon 美欧 周报**：
   - 产品数据（名称、价格、评分、排名）必须来自可验证的来源
   - 如无法获取实时数据，需明确标注数据来源和时效性
4. **宁可数据少，宁可报告短，也不能编造**

### 生成流程

每次生成周报前，必须完成以下核实：

1. 搜索采集原始数据
2. **第一遍核实**：将每条数据在原始来源中交叉验证
3. **第二遍核实**：随机抽查至少 30% 的数据，确认内容与来源一致
4. 剔除无法核实的数据
5. 生成 HTML 报告（`reports/{年份}-W{周数}.html`）
6. 同步生成 PDF 版本（`reports/{年份}-W{周数}.pdf`），使用以下命令：
   ```bash
   EDGE="C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe"
   "$EDGE" --headless --disable-gpu --print-to-pdf="<报告绝对路径>.pdf" --no-pdf-header-footer "file:///<报告绝对路径>.html"
   ```
7. 更新归档索引（`archive/index.html`）

### 数据标注

- 所有数据必须标注来源（如：CNIPA、sina财经、天眼查、Amazon 各站点等）
- 无法 100% 核实但有一定依据的数据，需标注置信度说明
- 每期报告末尾应有数据说明区域

### 质量承诺

如果本周可核实的有效数据过少（如不足 3 条），应如实说明情况，而不是通过编造来填充。

---

## 项目结构

```
weekly-reports/
├── templates/base.css          # 共享样式
├── index.html                  # 项目首页
├── patents/                    # 专利周报
│   ├── template.html           # 模板
│   ├── data/                   # JSON 原始数据
│   ├── reports/                # HTML 报告
│   └── archive/                # 归档索引
└── amazon/                     # Amazon 美欧周报
    ├── template.html
    ├── data/
    ├── reports/
    └── archive/
```

## 周报生成触发方式

- 固定每周六生成（用户触发）
- 用户也可以随时要求生成到当日为止的周报
- 触发后在对话中说"生成本周周报"或类似指令

## 技术约定

- HTML/CSS 纯静态，无外部依赖
- 系统字体栈：`-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", "Noto Sans SC", sans-serif`
- 专利周报：橙红暖色系 `#9a3412 / #ea580c / #fff7ed`
- Amazon 美欧周报：琥珀暖色系 `#92400e / #f59e0b / #fef3c7`
- 文件命名：`{年份}-W{周数}.html`，如 `2026-W25.html`
