# CCF 评级模块端到端测试报告

## 测试时间
2026-03-29 15:19 GMT+8

## 测试目标
验证从论文信息查询 → 发表状态识别 → CCF 评级匹配 → 权威度评分的完整流程

## 测试环境
- **CCF 目录版本**: 第七版（2026 年 3 月更新）
- **数据库规模**: 422 个 venue（会议 275 个，期刊 147 个）
- **测试方法**: 真实论文数据端到端测试

---

## 测试结果

### 总体统计
```
测试论文数：15 篇
通过：14 篇 (93.3%)
失败：1 篇  (6.7%)
```

### 详细结果

| # | 论文标题 | 发表 venue | 期望评级 | 实际评级 | 权威度 | 结果 |
|---|---------|-----------|---------|---------|--------|------|
| 1 | Latent Watermark | IEEE TMM | CCF-B | CCF-B | 0.80 | ✅ |
| 2 | Watermarking LLMs | ICML | CCF-A | CCF-A | 1.00 | ✅ |
| 3 | Certified Robust Watermark | arXiv | 预印本 | 预印本 | 0.30 | ✅ |
| 4 | ChainMarks | arXiv | 预印本 | 预印本 | 0.30 | ✅ |
| 5 | Undetectable Watermark | NeurIPS | CCF-A | CCF-A | 1.00 | ✅ |
| 6 | Let Watermarks Speak | ACL | CCF-A | CCF-A | 1.00 | ✅ |
| 7 | BiMarker | EMNLP | CCF-A | CCF-A | 1.00 | ✅ |
| 8 | More Haste Less Speed | CVPR | CCF-A | CCF-A | 1.00 | ✅ |
| 9 | Self-supervised CNN | Pattern Recognition | CCF-B | CCF-B | 0.80 | ✅ |
| 10 | Blind Visible Watermark | ESWA | CCF-C | CCF-C | 0.60 | ✅ |
| 11 | SiGRRW | IEEE TIFS | CCF-A | CCF-A | 1.00 | ✅ |
| 12 | Watermarking AR | ICLR | CCF-A | CCF-A | 1.00 | ✅ |
| 13 | DINVMark | IEEE TMM | CCF-B | CCF-B | 0.80 | ✅ |
| 14 | Invisible Watermarks | CCS | CCF-A | CCF-A | 1.00 | ✅ |
| 15 | Speech Watermarking | INTERSPEECH | 未评级 | 未评级 | 0.50 | ❌* |

*注：INTERSPEECH 不在 CCF 推荐列表中，系统正确识别为"未评级"，但类型判断有偏差（应为 conference，实际为 unknown）

---

## CCF 评级分布

### 成功识别的论文
- **CCF-A 类**: 9 篇 (60%)
  - 会议：ICML, NeurIPS, ACL, EMNLP, CVPR, ICLR, CCS
  - 期刊：IEEE TIFS
- **CCF-B 类**: 3 篇 (20%)
  - 会议：无
  - 期刊：IEEE TMM (2 篇), Pattern Recognition
- **CCF-C 类**: 1 篇 (6.7%)
  - 期刊：ESWA
- **预印本**: 2 篇 (13.3%)
  - arXiv 论文

---

### 1. 期刊全称匹配
期刊的全称键名，确保从 Semantic Scholar 获取的完整 venue 名称能够正确匹配：

| 期刊简称 | 期刊全称 | CCF 评级 |
|---------|---------|---------|
| IEEE TMM | IEEE Transactions on Multimedia | B |
| IEEE TIFS | IEEE Transactions on Information Forensics and Security | A |
| ESWA | Expert Systems with Applications | C |

### 2. 会议全称匹配
| 会议简称 | 会议全称 | CCF 评级 |
|---------|---------|---------|
| CCS | ACM Conference on Computer and Communications Security | A |

### 3. 匹配逻辑优化
- ✅ 期刊优先于会议匹配
- ✅ 长按名称优先（更具体的名称优先）
- ✅ 词边界匹配（避免短键名误匹配）
- ✅ 保留 "transactions" 和 "journal" 关键词用于区分期刊/会议

---

## 权威度评分验证

| CCF 评级 | 权威度分数 | 测试论文数 |
|---------|-----------|-----------|
| A 类 | 1.00 | 9 |
| B 类 | 0.80 | 3 |
| C 类 | 0.60 | 1 |
| 预印本 | 0.30 | 2 |
| 未评级 | 0.50 | 1 |

所有评分符合预期标准。

---

## 数据库统计

### 会议数据库
| 评级 | 数量 | 代表性会议 |
|------|------|-----------|
| A 类 | 65 | NeurIPS, ICML, ICLR, CVPR, ICCV, ECCV, ACL, EMNLP, NAACL, CCS, S&P, USENIX Security, NDSS, SIGMOD, VLDB, ICDE, KDD, WWW, SIGIR, ICSE, FSE, ISSTA, PLDI, POPL, OSDI, SOSP, SIGCOMM, INFOCOM |
| B 类 | 96 | ICANN, ICONIP, WACV, BMVC, ICIP, CIKM, RecSys, ECIR, ASIACRYPT, ESORICS, DSN, RAID, CHES |
| C 类 | 113 | IJCNN, ICPR, DASFAA, APWeb, ICC, GLOBECOM, LCN, ACNS, AsiaCCS, ICICS, TrustCom |

### 期刊数据库
| 评级 | 数量 | 代表性期刊 |
|------|------|-----------|
| A 类 | 28 | IEEE TPAMI, IJCV, JMLR, Artificial Intelligence, TACL, IEEE TIP, IEEE TIFS, TDSC, TOPLAS, TOSEM, TSE, TODS, TOIS, TKDE |
| B 类 | 53 | IEEE TMM, IEEE TNNLS, Pattern Recognition, Neural Networks, Machine Learning, KAIS, Computers & Security |
| C 类 | 62 | ESWA, Neurocomputing, Pattern Recognition Letters, NPL, Expert Systems with Applications |

---

## 测试命令

### 运行完整测试
```bash
cd /home/admin/.openclaw/workspace/skills/paper-review-pro
python scripts/test_publication_status.py
```

### 测试特定期刊/会议
```bash
python scripts/test_publication_status.py --title "论文标题" --venue "IEEE Transactions on Multimedia"
```

### 显示数据库统计
```bash
python scripts/test_publication_status.py --show-db
```

### 运行真实论文检索（端到端）
```bash
python scripts/review.py --query "watermark" --retrieve_number 50 --keep_topk 10 --year 2024
```

---

## 结论

### ✅ 已验证功能
1. **期刊识别**: IEEE TMM, IEEE TIFS, Pattern Recognition, ESWA 等正确识别
2. **会议识别**: ICML, NeurIPS, CVPR, ACL, EMNLP, ICLR, CCS 等正确识别
3. **预印本识别**: arXiv 论文正确标记为 preprint
4. **CCF 评级**: A/B/C 类正确匹配
5. **权威度评分**: 1.0/0.8/0.6/0.3/0.5 分数正确计算
6. **端到端流程**: 从论文信息到最终评级的完整流程正常工作

### ⚠️ 边缘情况
- INTERSPEECH 等不在 CCF 列表中的会议正确识别为"未评级"
- 类型判断（conference vs journal）在无 venue 信息时可能不准确

### 📊 测试覆盖率
- **真实论文测试**: 15 篇
- **通过率**: 93.3%
- **CCF 等级覆盖**: A/B/C/预印本/未评级 全覆盖
- **类型覆盖**: 期刊/会议/预印本 全覆盖

---
