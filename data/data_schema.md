# 数据集格式说明 (Data Schema)

本目录存储了 ChP2025-QA Benchmark 的核心数据文件。数据文件采用 JSON Lines (`.jsonl`) 格式，每一行代表一条完整的问答评测样本。

## 字段说明

对于 `.jsonl` 文件中的每一行 JSON 对象，包含以下核心字段：

| 字段名称 (Field) | 类型 (Type) | 必填 | 描述 (Description) |
|------------------|-------------|------|--------------------|
| `id` | String | 是 | 样本的唯一标识符。 |
| `question` | String | 是 | 用户提出的问题文本。 |
| `question_type` | String | 是 | 问题类型，包括但不限于：`entity_to_test` (品种查检测项), `test_to_method` (检测项查方法), `method_to_condition` (方法查条件) 等。 |
| `is_answerable` | Boolean | 是 | 核心字段：表示该问题是否可以根据《中国药典》当前库的知识得出准确答案 (`true` 或 `false`)。用以评测模型的拒答能力。 |
| `gold_evidence` | List[String] | 是 | 黄金证据：解决该问题所必须依据的具体药典条目、段落或路径节点标识。 |
| `support_summary` | String | 否 | 支持该答案的总结性文本或人工标注的标准参考答案。 |
| `answer` | String | 否 | 精确答案文本（针对 `is_answerable=true` 的样本）。不可答样本该字段通常为空或特定拒答回复。 |

## 样本示例 (Example)

### 1. 可回答样本示例 (Answerable = True)
```json
{
  "id": "q_001",
  "question": "阿司匹林肠溶片的溶出度测定中，缓冲液的pH值是多少？",
  "question_type": "method_to_condition",
  "is_answerable": true,
  "gold_evidence": ["阿司匹林肠溶片->溶出度->酸中溶出量->pH 6.8 磷酸盐缓冲液"],
  "support_summary": "根据药典规定，阿司匹林肠溶片溶出度检查分为酸中和缓冲液中两步，缓冲液阶段应使用pH 6.8的磷酸盐缓冲液。",
  "answer": "pH 6.8"
}
```

### 2. 不可回答样本示例 (Answerable = False)
```json
{
  "id": "q_002",
  "question": "注射用头孢曲松钠的重金属限度是多少？",
  "question_type": "entity_test_method_condition",
  "is_answerable": false,
  "gold_evidence": [],
  "support_summary": "药典2025版注射用头孢曲松钠项下并未规定‘重金属’检查项，因此该问题无法基于现有规范回答。",
  "answer": "根据《中国药典》当前条款，注射用头孢曲松钠项下未收载重金属检查项，无法提供限度。"
}
```

## 注意事项
* 在进行模型评测时，`is_answerable=false` 的样本主要用于评估指标中的 **UA-Acc (Unanswerable Accuracy)**。
* 指标 `Cite-P (Citation Precision)` 和 `Evid-R (Evidence Recall)` 的计算强依赖于模型生成的证据与 `gold_evidence` 字段的对比。
