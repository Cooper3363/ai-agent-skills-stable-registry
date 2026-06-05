# 状态汇总

## 可交付平台：13 个

| Skill ID | 业务包 | 主要用途 | 接入备注 |
| --- | --- | --- | --- |
| `marketing_copy_pack` | 营销内容 | 生成营销文案草稿 | 不自动发布 |
| `daily_weekly_metrics_reporter` | 经营报表 | 生成日报、周报、月报 | 不替代审计或重大经营决策 |
| `metric_exception_classifier` | 经营报表 | 异常指标分类和核查建议 | 不做确定归因 |
| `faq_answer_with_citations` | 客服知识库 | 带引用的知识库问答 | citation 必须来自实际命中知识库 |
| `support_ticket_classifier` | 客服知识库 | 工单分类和转人工建议 | 不直接处理工单 |
| `structured_campaign_brief` | 营销内容 | 活动 brief 结构化 | 不自动发布或改预算 |
| `structured_metric_summary` | 经营报表 | 单项指标摘要 | 与完整报告生成区分 |
| `crm_note_structurer` | 销售跟进 | CRM 跟进记录结构化 | 默认无写入 CRM 权限 |
| `support_reply_guardrail` | 客服知识库 | 客服回复风险检查 | 只检查不处置 |
| `marketing_compliance_guard` | 营销内容 | 营销文案合规风险检查 | 不替代法务或广告平台审核 |
| `support_pii_redactor` | 客服知识库 | 中文客服 PII 脱敏 | 不做身份判断；禁止外部上传 |
| `support_kb_doc_cleaner` | 客服知识库 | 知识库前置文本清洗 | 仅本地/mock 文本；不接 Platform/API 或云 OCR |
| `expense_invoice_parser` | 经营报表/财票 | 本地/mock 票据字段抽取 | `not_tax_advice=true`；不做报销/税务/审计结论 |

## 组件池：5 个

| 组件 | 来源 | 当前定位 |
| --- | --- | --- |
| `seo_keyword_extractor` | KeyBERT | 营销内容包关键词候选组件，不独立 L3 |
| `data_quality_rules_check` | Great Expectations | 报表数据质检前置组件，不独立输出经营结论 |
| `partner_page_snapshot` | Crawlee | mock/组件定位，暂不做真实抓取型封装 |
| `receipt_line_ocr_extractor` | PaddleOCR | 票据 OCR 结果后处理组件，不代表真实 OCR 通过 |
| `sales_speaker_timeline` | WhisperX | 会议说话人时间线整理组件，不代表真实 diarization 通过 |

## 暂缓或观察

| 候选 | 原因 |
| --- | --- |
| Jina Reader / `lead_web_brief` | 需真实网页补测，不进入当前 L3 |
| Rasa | P0 暂缓，中文意图分类依赖训练集且维护状态需观察 |
| browser-use | 浏览器自动化、目标站 ToS、反爬和登录边界风险 |
| Composio | OAuth、邮件/CRM 写入动作和第三方 API 条款风险 |
| PaddleOCR 完整 OCR Skill | 真实 OCR 模型、票据图片隐私和依赖边界未通过 Harness；当前只保留后处理组件 |
| WhisperX 完整 diarization Skill | 真实 diarization/音频对齐、pyannote/HF user agreement、录音隐私未通过 Harness；当前只保留时间线整理组件 |

## 下一批进行中

| 环节 | 状态 |
| --- | --- |
| 下一批 Top 10 许可证复核 | 已完成首轮：6 个送测，2 个需模型/依赖补充，1 个真实抓取暂缓，1 个需法务/安全复核 |
| 下一批 Top 10 L2 用例模板 | 已完成预案 |
| 下一批 6 个 L2 simulated 测试 | 已完成：4 个可送封装，2 个需受控真实 Harness 补测 |
| 下一批 4 个 L3 草案封装 | 已派发封装专员：`support_response_eval_suite`, `hr_resume_privacy_masker`, `support_rag_eval_runner`, `contract_section_partitioner` |
| Camelot / `quote_pdf_table_extractor` | 需受控本地 PDF Harness 补测，补测前不进入 L3 |
| Whisper / `sales_meeting_transcriber` | 需受控短音频 Harness 补测，补测前不进入 L3 |
| PaddleOCR / `receipt_line_ocr_extractor` | 受限 L2 simulated 已完成：L2 通过但仅作为组件；真实 OCR 不放行 |
| WhisperX / `sales_speaker_timeline` | 受限 L2 simulated 已完成：L2 通过但仅作为组件；真实 diarization 不放行 |
| P1 候选池继续发掘 | 进行中 |
