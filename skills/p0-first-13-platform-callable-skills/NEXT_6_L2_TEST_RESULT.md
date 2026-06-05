# 下一批 6 个许可证通过候选 L2 simulated 测试结果

日期：2026-06-03

本文件记录下一批 6 个已通过许可证产品筛选候选的 L2 simulated 中文业务用例测试结果。本轮每个候选覆盖 3 个中文业务用例，共 18 个用例。

## 执行边界

- 未安装依赖。
- 未访问外网。
- 未读取真实文件。
- 未运行真实 PDF、音频、模型或向量库。
- DeepEval/Ragas 只评测 mock question/context/answer/reply。
- Presidio 只处理 mock 简历文本。
- Camelot 只处理 mock PDF 表格文本或 mock_extracted_rows。
- Whisper 只处理 mock transcript_text 和 audio_quality_notes。
- Unstructured 只处理 mock 合同文本。

## 总体结论

| 结论 | 数量 | Skill |
| --- | ---: | --- |
| L2 通过，可进入下一步封装候选 | 4 | `support_response_eval_suite`, `hr_resume_privacy_masker`, `support_rag_eval_runner`, `contract_section_partitioner` |
| 需受控真实 Harness 补测 | 2 | `quote_pdf_table_extractor`, `sales_meeting_transcriber` |

## 结果总表

| 候选 | Skill ID | 输出结构稳定性 | 中文可用性 | 权限边界 | 人工复核触发 | L2 结论 | 建议下一步 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DeepEval | `support_response_eval_suite` | 高 | 高 | 只评测 mock 回复，不接真实知识库或外部 LLM 平台 | 低分、高风险、退款、账号安全、隐私风险 | L2 通过 | 送 L3 封装 |
| Microsoft Presidio | `hr_resume_privacy_masker` | 高 | 中高 | 只处理 mock 简历文本，不读真实简历、不上传外部服务 | 高敏 PII、推荐人联系方式、实体低置信度 | L2 通过 | 送 L3 封装 |
| Camelot | `quote_pdf_table_extractor` | 中高 | 中高 | 只处理 mock 表格文本/行，不读真实 PDF、不 OCR | 金额不一致、列错位、低置信度 | 需补测 | 受控本地 PDF Harness 补测后再决定是否封装 |
| Whisper | `sales_meeting_transcriber` | 中高 | 高 | 只处理 mock transcript，不处理或上传真实音频 | 音质差、价格合同承诺、低置信片段 | 需补测 | 受控短音频真实 Harness 补测后再决定是否封装 |
| Ragas | `support_rag_eval_runner` | 高 | 高 | 只评测 mock question/context/answer，不接真实向量库或外部平台 | 幻觉、无引用、低上下文相关性 | L2 通过 | 送 L3 封装 |
| Unstructured | `contract_section_partitioner` | 高 | 高 | 只处理 mock 合同文本，不读真实合同、不接 API/云 OCR、不做法律结论 | 自动续费、违约、保密、争议解决、高风险条款 | L2 通过 | 送 L3 封装 |

## 通过项封装要求

### `support_response_eval_suite`

- 来源项目：DeepEval。
- 推荐定位：客服回复质量/安全评测套件。
- 固定输出字段：`overall_score`, `criteria_scores`, `failed_checks`, `risk_flags`, `suggested_fixes`, `human_review_required`, `eval_notes`。
- 封装边界：只评测回复和 mock 上下文，不自动修改线上回复，不访问外部知识库。
- 重点人工复核：退款/赔偿承诺、账号安全绕验证、隐私风险、低分高风险回复。

### `hr_resume_privacy_masker`

- 来源项目：Microsoft Presidio。
- 推荐定位：HR 简历隐私脱敏组件/Skill。
- 固定输出字段：`redacted_text`, `entities`, `preserved_fields`, `risk_level`, `masking_notes`, `manual_review_required`。
- 封装边界：只处理 mock 或本地文本；不读真实简历文件，不上传外部服务。
- 重点人工复核：身份证、详细住址、出生日期、第三方推荐人联系方式、实体低置信度。

### `support_rag_eval_runner`

- 来源项目：Ragas。
- 推荐定位：客服 RAG 回答评测组件/Skill。
- 固定输出字段：`faithfulness_score`, `answer_relevance`, `context_precision`, `citation_coverage`, `failure_reasons`, `eval_summary`, `human_review_required`。
- 封装边界：只评测输入的 question/context/answer；不接真实向量库，不默认调用外部平台。
- 重点人工复核：幻觉、无引用、上下文不相关但答案自信、missing_knowledge。

### `contract_section_partitioner`

- 来源项目：Unstructured。
- 推荐定位：合同条款分区清洗 Skill。
- 固定输出字段：`sections`, `section_types`, `clause_summaries`, `risky_sections`, `missing_sections`, `quality_warnings`, `manual_review_required`, `not_legal_advice`。
- 封装边界：只处理 mock 或本地合同文本；不读取真实合同文件，不接云 OCR/API，不输出法律结论。
- 重点人工复核：自动续费、违约金、隐私/保密、争议解决、付款/SLA 高风险条款。

## 需补测项

### `quote_pdf_table_extractor`

- 当前原因：本轮只验证下游结构化与风险字段，未验证 Camelot 真实 PDF 抽表能力。
- 建议补测：受控本地小样本 PDF Harness。
- 补测前状态：需补测，不进入 L3 封装。
- 通过后再判断：是否可作为报价 PDF 表格抽取 Skill 进入封装。

### `sales_meeting_transcriber`

- 当前原因：本轮只验证 mock transcript 结构化，未验证 Whisper 真实音频转写能力。
- 建议补测：受控本地短音频 Harness。
- 补测前状态：需补测，不进入 L3 封装。
- 通过后再判断：是否可作为销售会议转写 Skill 进入封装。

## 指挥中心决策记录

- 4 个 L2 通过候选送封装专员做 L3 草案。
- Camelot 和 Whisper 暂不送封装。
- Camelot/Whisper 的真实 Harness 补测需由指挥中心单独批准测试样本、权限边界和执行窗口。
- DeepEval/Ragas 后续封装需明确 evaluator 范围：优先做结构化评测包装；如接 LLM evaluator，需声明模型/API key、成本、日志和失败阈值。
