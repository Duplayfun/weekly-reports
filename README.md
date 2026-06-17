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

每周六在对话中触发 AI 生成（若当天没空可延后）：

1. AI 采集最新数据（专利 + Amazon）
2. 数据写入 `data/`（JSON）
3. HTML 报告写入 `reports/`
4. 更新 `archive/index.html`
5. git push → GitHub Pages 自动发布

## 在线查看

部署后访问：`https://<username>.github.io/weekly-reports/`
