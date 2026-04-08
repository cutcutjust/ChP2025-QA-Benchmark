# ChP2025-QA: A Structured Question Answering Benchmark for Chinese Pharmacopoeia 2025

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data License](https://img.shields.io/badge/Data%20License-CC%20By%204.0-red.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

[**中文版说明 (Chinese)**](#中文说明) | [**English**](#english-introduction)

---

## 中文说明

**ChP2025-QA** 是首个面向《中国药典》(2025版) 的结构化知识问答基准测试集。在医疗法规与标准文档等高约束场景下，问答系统必须做到**证据可追溯**、**严格核验**以及在证据不足时**稳定拒答**。

本基准共包含 **425** 条精心标注的问答样本（其中可回答样本 278 条，不可回答样本 147 条），并对“品种-检测项-方法-条件/限度”之间的多跳结构化对齐能力进行重点考察。

### 🏆 排行榜 (Leaderboard)

基于本数据集的基线评测结果如下（更多消融实验结果详见论文）：

| Method / Baseline | Exact Match (EM) | F1 Score | Citation Precision | Evidence Recall | Unanswerable Acc (UA-Acc) |
|-------------------|------------------|----------|--------------------|-----------------|---------------------------|
| Base LLM          | 11.9%            | 37.0%    | 0.0%               | 0.0%            | 26.5%                     |
| Text RAG          | 20.9%            | 33.4%    | 16.7%              | 19.2%           | 91.8%                     |
| Graph RAG         | 24.8%            | 38.3%    | 17.9%              | 19.7%           | 86.4%                     |
| Hier RAG          | 42.1%            | 59.4%    | 44.4%              | 42.7%           | 72.8%                     |
| **Ours (Method 3)**| **56.8%**        | **69.4%**| **64.4%**          | **67.5%**       | **76.2%**                 |

### 📂 仓库结构

```text
ChP2025-QA-Benchmark/
├── data/                       # 测试基准数据目录
│   ├── benchmark_candidates_all.jsonl  # 完整评测集
│   └── data_schema.md          # 数据字段说明
├── baselines/                  # 基准模型代码 (待移入)
├── evaluation/                 # 自动化评测脚本 (待移入)
├── requirements.txt            # 环境依赖包
└── README.md                   # 项目说明
```

### 🚀 快速开始

1. **环境准备**
   ```bash
   git clone https://github.com/your-username/ChP2025-QA-Benchmark.git
   cd ChP2025-QA-Benchmark
   pip install -r requirements.txt
   ```

2. **数据加载示例**
   ```python
   import json
   with open('data/benchmark_candidates_all.jsonl', 'r', encoding='utf-8') as f:
       for line in f:
           data = json.loads(line)
           print(f"Question: {data['question']}")
           print(f"Is Answerable: {data['is_answerable']}")
           break
   ```

### 📄 引用 (Citation)

如果您在研究中使用了本数据集或代码，请引用我们的论文：
```bibtex
@article{YourName2026ChP2025QA,
  title={Structured Question Answering over the Chinese Pharmacopoeia: Benchmark Construction and Hierarchical Path RAG with Evidence Verification},
  author={Your Name and Co-authors},
  journal={arXiv preprint arXiv:XXXX.XXXXX},
  year={2026}
}
```

---

## English Introduction

**ChP2025-QA** is the first structured knowledge question-answering benchmark designed for the *Chinese Pharmacopoeia (2025 Edition)*. In highly constrained scenarios such as medical regulations and standard documents, QA systems must ensure **evidence traceability**, **strict verification**, and **stable refusal to answer (abstention)** when evidence is insufficient.

This benchmark contains **425** meticulously annotated QA samples (278 answerable and 147 unanswerable), focusing on evaluating the multi-hop structured alignment capabilities across "drug - test item - method - condition/limit".

*(For Leaderboard and usage, please refer to the sections above).*
