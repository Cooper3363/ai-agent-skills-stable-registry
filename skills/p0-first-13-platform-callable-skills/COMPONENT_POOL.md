# 组件池

这些对象已经通过一定程度的研究或 simulated L2，但当前不作为 13 个独立 Skill 入包。它们只在后续组合包或前置流程中使用。

| 组件 ID | 来源项目 | 业务包 | 当前状态 | 使用方式 | 不独立封装原因 |
| --- | --- | --- | --- | --- | --- |
| `seo_keyword_extractor` | KeyBERT | 营销内容生产包 | L2 通过但仅组件定位 | 嵌入 `structured_campaign_brief` 或内容生产流程，给关键词候选、风险词和词典命中 | 中文分词、停用词、行业词典决定质量；独立输出商业价值不够稳定 |
| `data_quality_rules_check` | Great Expectations | 经营报表分析包 | L2 通过但仅组件定位 | 作为 `daily_weekly_metrics_reporter` 或 `structured_metric_summary` 的数据质检前置步骤 | 只输出数据质量，不应独立输出经营分析结论 |
| `partner_page_snapshot` | Crawlee | 销售跟进助手包 | L2 通过但仅 mock/组件定位 | 只在授权环境下做公开页面快照组件，先用于合作方调研流程草案 | 真实抓取涉及目标站 ToS、robots、联系人采集、代理和反爬边界 |
| `receipt_line_ocr_extractor` | PaddleOCR | 经营报表/财票分析包 | L2 通过但仅组件定位 | 承接 `mock_ocr_text` 或上游 OCR lines，做票据行项目结构化、金额校验、低置信提示和 PII 标记 | 真实 OCR 未测；不能证明图片识别、模型下载或 OCR Harness 通过 |
| `sales_speaker_timeline` | WhisperX | 销售跟进助手包 | L2 通过但仅组件定位 | 承接上游 diarized segments，整理说话人时间线、关键时刻、行动项归属和低置信片段 | 真实 diarization/音频对齐未测；pyannote/HF user agreement、token、模型和录音隐私边界需另核 |

## 使用规则

- 组件池不计入 13 个正式 Skill 数量。
- 组件池不能直接交给平台同事作为可接入 Skill。
- 组件要进入平台，必须挂靠到清晰的业务 Skill 或重新通过许可证、L2、封装、平台调用验收。
- `receipt_line_ocr_extractor` 不能被描述为真实 OCR 能力已通过；只能接收上游 OCR 文本/行。
- `sales_speaker_timeline` 不能被描述为真实说话人识别已通过；只能接收上游 diarized segments。
