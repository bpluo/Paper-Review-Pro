---
name: paper-reviewer-pro
description: 高精度论文检索与检阅系统，支持多源检索、智能筛选、结构化摘要、BibTeX 导出、CCF 评级与综合评分
license: MIT
clawhub:
  slug: paper-reviewer-pro
  repo: yourname/skills
  repoPath: skills/paper-reviewer-pro
  ref: main
  version: v1.0.0
  autoEnable: true
---

# Paper Review Pro - 论文检阅系统

## 📖 简介

Paper Review Pro 是一款面向科研工作者的**智能论文检索与分析工具**。从 arXiv 和 Semantic Scholar 多源检索论文，自动筛选核心文献、生成结构化摘要、评估论文权威性，输出完整的研究领域分析报告。

**适用场景**：

- 🔍 **领域入门** — 快速了解新领域的核心文献与关键学者
- 📚 **文献综述** — 系统性文献综述（SLR）的自动化辅助
- 💡 **方向探索** — 发现交叉领域和潜在创新点
- 📊 **前沿追踪** — 按年份过滤，追踪最新进展
- 📑 **文献管理** — 一键导出 BibTeX，无缝导入 Zotero/Mendeley

---

## ✨ 核心优势

| 特性            | 说明                                                    |
| --------------- | ------------------------------------------------------- |
| **多源检索**    | 同时检索 arXiv + Semantic Scholar，覆盖预印本与正式发表 |
| **智能评分**    | 相关度 × 权威度综合评分，精准识别高价值论文             |
| **CCF 评级**    | 内置 422 个 CCF 推荐 venue，自动标注 A/B/C 类           |
| **AI 摘要**     | LLM 自动提取研究问题、方法、结论、创新点四要素          |
| **领域扩展**    | 基于 Top-K 论文生成扩展检索词，发现子领域               |
| **BibTeX 导出** | 自动生成标准 BibTeX，一键导入 Zotero 等工具             |
| **完整报告**    | 自动生成研究领域分析报告，含统计与趋势                  |
| **高健壮性**    | 多层错误回退 + Timeout 保护，极端情况不中断             |

---

## 🚀 快速开始

### 安装

```bash
openclaw skills install paper-review-pro
```

### 基本检索

```bash
cd ~/.openclaw/workspace/skills/paper-review-pro
python scripts/review.py --query "LLM reasoning" --retrieve_number 20 --keep_topk 5 --year 2024
```

### 参数说明

| 参数                    | 必需 | 默认值 | 说明                       |
| ----------------------- | ---- | ------ | -------------------------- |
| `--query`               | ✅   | -      | 检索关键词                 |
| `--retrieve_number`     | ❌   | 10     | 初始检索数量               |
| `--keep_topk`           | ❌   | 3      | 保留核心论文数量           |
| `--year`                | ❌   | 2025   | 截止年份（检索该年及之后） |
| `--expand_query_number` | ❌   | 2      | 每个扩展词保留的论文数量   |

### 输出内容

| #   | 输出项         | 格式     | 位置                                    |
| --- | -------------- | -------- | --------------------------------------- |
| 1   | **Top-K 论文** | 排序列表 | 终端直接输出                            |
| 2   | **扩展检索**   | 补充结果 | 终端输出                                |
| 3   | **BibTeX**     | `.bib`   | `~/.openclaw/paper-review-pro/papers/`  |
| 4   | **研究报告**   | Markdown | `~/.openclaw/paper-review-pro/reports/` |

---

## 📦 功能详解

### 1. 多源检索与去重

**检索源**：

- **arXiv** - 预印本为主，覆盖最新研究
- **Semantic Scholar** - 正式发表为主，含引用信息

**去重机制**：基于论文标题和 DOI 进行本地去重，避免重复结果。

### 2. 综合评分系统

```
Score = Relevance × (1 - weight) + Authority × weight
```

| 参数          | 说明                                         | 默认     |
| ------------- | -------------------------------------------- | -------- |
| **Relevance** | 查询词与标题/摘要的语义匹配度                | 算法计算 |
| **Authority** | 基于发表状态与 CCF 评级的权威度              | 查表获得 |
| **weight**    | 权威度权重，可通过 `--authority-weight` 调节 | 0.3      |

**权威度评分**：

| 等级         | 说明                                     | 分值 |
| ------------ | ---------------------------------------- | ---- |
| CCF-A        | 顶会/顶刊（NeurIPS, ICML, CVPR, ACL...） | 1.0  |
| CCF-B        | 优秀会议/期刊                            | 0.8  |
| CCF-C        | 良好会议/期刊                            | 0.6  |
| 已发表未评级 | 有 venue 但未入 CCF 目录                 | 0.5  |
| Preprint     | arXiv 等预印本                           | 0.3  |

### 3. CCF 评级查询

内置 **中国计算机学会推荐目录**，覆盖 422 个 venue（会议 275 / 期刊 147）。

**代表性 A 类收录**：

| 领域  | 会议                           | 期刊              |
| ----- | ------------------------------ | ----------------- |
| AI/ML | NeurIPS, ICML, ICLR, CVPR, ACL | TPAMI, IJCV, JMLR |
| 安全  | CCS, S&P, USENIX Security      | TIFS              |
| 系统  | SIGMOD, VLDB, SOSP             | TOS, TKDE         |

### 4. 结构化摘要

LLM 自动提取论文四要素：

| 要素            | 说明                      |
| --------------- | ------------------------- |
| 🔬 **研究问题** | 论文要解决的核心问题      |
| ⚙️ **方法**     | 采用的技术路线 / 算法框架 |
| 📊 **结论**     | 主要发现与实验结果        |
| 💡 **创新点**   | 与已有工作的关键区别      |

### 5. 领域扩展

基于 Top-K 论文内容分析，自动生成语义扩展词，发现：

- 📌 **相关子领域** — 拓宽视野，避免盲区
- 🔄 **替代方案** — 对比不同技术路线
- 🔗 **交叉应用** — 跨领域迁移机会

### 6. BibTeX 导出

自动生成标准 BibTeX 文件，兼容 Zotero/Mendeley/EndNote。

**导入 Zotero：** `文件 → 导入 → 从文件导入` → 选择生成的 `.bib` 文件

---

## ⚙️ 配置与参数

### 配置文件

位置：`~/.openclaw/paper-review-pro/config.json`

```json
{
  "default_n": 10,
  "default_k": 2,
  "min_year": 2025,
  "default_p": 2,
  "authority_weight": 0.3,
  "llm": {
    "enabled": true,
    "provider": "gateway",
    "gateway_url": "http://localhost:14940",
    "gateway_model": "dashscope/qwen3.5-plus"
  }
}
```

### 快速配置

```bash
python scripts/config.py --default_n 20 --default_k 3 --min_year 2024 --authority-weight 0.3
```

### 命令行参数

#### 基本参数

| 参数                    | 必需 | 默认 | 说明                   |
| ----------------------- | ---- | ---- | ---------------------- |
| `--query`               | ✅   | —    | 检索关键词             |
| `--retrieve_number`     | ❌   | 10   | 初始检索数量           |
| `--keep_topk`           | ❌   | 3    | 保留核心论文数         |
| `--year`                | ❌   | 2025 | 截止年份（含该年之后） |
| `--expand_query_number` | ❌   | 2    | 每扩展词保留数         |

#### 高级参数

| 参数                 | 说明                    | 默认 |
| -------------------- | ----------------------- | ---- |
| `--no-bibtex`        | 禁用 BibTeX 导出        | 启用 |
| `--no-authority`     | 禁用权威度评分          | 启用 |
| `--authority-weight` | 权威度权重 (0.0–1.0)    | 0.3  |
| `--use-api`          | 通过 API 在线查发表状态 | 禁用 |
| `--no-llm`           | 禁用 LLM 摘要           | 启用 |

---

## 🛡️ 错误处理与回退机制

本系统设计有多层错误处理机制，确保在各种异常情况下仍能稳定运行。

### 1. arXiv 检索回退

```
API 请求 (首选) → 网页爬取 (回退) → 跳过 arXiv
```

### 2. LLM 功能回退

```
Gateway API (首选) → Dashscope API (备用) → 规则提取 Fallback
```

### 3. 发表状态查询回退

```
在线 API → 本地数据库 → 标记"未评级"
```

### 4. 卡死检测保护

**TimeoutMonitor** 监控所有关键步骤：

- 超时阈值：600 秒（10 分钟）
- 监控点：检索、过滤、评分、扩展、导出、报告生成
- 超时行为：自动终止程序并输出错误信息

### 5. 网络请求重试

所有外部 API 请求均支持自动重试：**3 次尝试，指数退避（1s → 2s → 4s）**。

---

## 📋 使用示例

| 用例             | 命令                                                                                                      | 说明       |
| ---------------- | --------------------------------------------------------------------------------------------------------- | ---------- |
| **标准检索**     | `python scripts/review.py --query "transformer attention" --retrieve_number 20 --keep_topk 5 --year 2020` | 基础用法   |
| **纯内容分析**   | `python scripts/review.py --query "novel architecture" --keep_topk 5 --no-authority`                      | 忽略权威度 |
| **系统性综述**   | `python scripts/review.py --query "deep learning survey" --keep_topk 10 --authority-weight 0.5`           | 权威度优先 |
| **快速测试**     | `python scripts/review.py --query "quick test" --retrieve_number 5 --no-llm`                              | 禁用 LLM   |
| **精确发表信息** | `python scripts/review.py --query "recent paper" --keep_topk 5 --use-api`                                 | 在线查状态 |

---

## 🧪 测试与验证

### CCF 评级

```bash
# 完整测试套件
python scripts/test_publication_status.py

# 测试特定论文
python scripts/test_publication_status.py --title "论文标题" --venue "IEEE Transactions on Multimedia"

# 查看数据库统计
python scripts/test_publication_status.py --show-db
```

### BibTeX 导出

```bash
python scripts/core/bibtex.py --title "Test Paper" --authors "John Doe" --year 2025 --publication "CVPR" --ccf-rank A
```

---

## 📚 参考文档

详细模块说明请参考 `reference/` 目录：

| 文档                                                                 | 说明                           |
| -------------------------------------------------------------------- | ------------------------------ |
| [`reference/LLM_INTEGRATION.md`](reference/LLM_INTEGRATION.md)       | LLM 集成（摘要生成、查询扩展） |
| [`reference/BIBTEX_EXPORT.md`](reference/BIBTEX_EXPORT.md)           | BibTeX 导出模块                |
| [`reference/PUBLICATION_STATUS.md`](reference/PUBLICATION_STATUS.md) | 发表状态与 CCF 评级            |
| [`reference/SCORING_SYSTEM.md`](reference/SCORING_SYSTEM.md)         | 综合评分系统                   |
| [`reference/BUGFIXES.md`](reference/BUGFIXES.md)                     | 重要修复记录                   |

---

## ⚠️ 注意事项

| #   | 注意点             | 详情                                                                            |
| --- | ------------------ | ------------------------------------------------------------------------------- |
| 1   | **CCF 数据库**     | 覆盖主流 CS 会议/期刊。扩展请编辑 `scripts/core/publication_status.py` 评级字典 |
| 2   | **BibTeX 路径**    | 输出至 `~/.openclaw/paper-review-pro/papers/bibtex_{query}_{timestamp}.bib`     |
| 3   | **权威度权重建议** | 探索研究 0.2–0.3 … 综述 0.4–0.5 … 纯内容 `--no-authority`                       |
| 4   | **在线查状态**     | `--use-api` 更准确，但每篇增加 2–5 秒                                           |
| 5   | **arXiv 限速**     | API 间隔 ≥3 秒。网页爬取回退超时 60 秒                                          |
| 6   | **LLM 配置**       | 首次使用前确保 Gateway 或 Dashscope 已配好。未配置自动降级为规则 Fallback       |

---

## 📝 更新日志

详见 [`CHANGELOG.md`](CHANGELOG.md) 或项目发布说明。

---

**版本**: 2026-05-15
**许可**: MIT  
**维护**: OpenClaw Community
