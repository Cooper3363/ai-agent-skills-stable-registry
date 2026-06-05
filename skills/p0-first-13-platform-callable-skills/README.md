# P0 首批 13 个 AI.Skills 资料包

核验日期：2026-06-03

这个目录用于集中存放 P0 首批可给平台团队查看和接入评估的 13 个 AI.Skills。它是资料包和交接包，不是生产上线目录。

## 当前数量

| 口径 | 数量 | 说明 |
| --- | ---: | --- |
| 可交付平台 | 13 | 已通过平台调用验收，可交给平台同事做接入前真实 wrapper smoke test |
| 待平台调用验收 | 0 | 当前无待验收项 |
| 本包正式 Skill 总数 | 13 | 不包含组件池和暂缓观察项 |

## 业务包分布

| 业务包 | Skill 数量 | Skill |
| --- | ---: | --- |
| 客服知识库助手包 | 5 | `faq_answer_with_citations`, `support_ticket_classifier`, `support_reply_guardrail`, `support_pii_redactor`, `support_kb_doc_cleaner` |
| 营销内容生产包 | 3 | `marketing_copy_pack`, `structured_campaign_brief`, `marketing_compliance_guard` |
| 销售跟进助手包 | 1 | `crm_note_structurer` |
| 经营报表/财票分析包 | 4 | `daily_weekly_metrics_reporter`, `metric_exception_classifier`, `structured_metric_summary`, `expense_invoice_parser` |

## 快速入口

- [INDEX.md](INDEX.md)：13 个 Skill 快速索引表。
- [STATUS_SUMMARY.md](STATUS_SUMMARY.md)：可交付、组件池、暂缓项汇总。
- [SMOKE_TEST_PLAN.md](SMOKE_TEST_PLAN.md)：13 个 Skill 的平台接入前 smoke test 清单。
- [NEXT_TOP10_LICENSE_REVIEW.md](NEXT_TOP10_LICENSE_REVIEW.md)：下一批 Top 10 候选的许可证与商用风险复核。
- [NEXT_TOP10_L2_TEST_PLAN.md](NEXT_TOP10_L2_TEST_PLAN.md)：下一批 Top 10 候选的 L2 中文业务用例模板预案。
- [NEXT_6_L2_TEST_RESULT.md](NEXT_6_L2_TEST_RESULT.md)：下一批 6 个许可证通过候选的 L2 simulated 测试结果。
- [PADDLEOCR_WHISPERX_RESTRICTED_L2_RESULT.md](PADDLEOCR_WHISPERX_RESTRICTED_L2_RESULT.md)：PaddleOCR 与 WhisperX 的受限 L2 simulated 结果。
- [COMPONENT_POOL.md](COMPONENT_POOL.md)：当前不作为独立 Skill 的组件候选。
- [COMBINATION_PACK_PLAN.md](COMBINATION_PACK_PLAN.md)：组件池组合包封装方案。
- [skills/](skills)：每个 Skill 的独立说明和结构化元数据。

## 使用边界

- 所有结论均为产品筛选阶段判断，不替代正式法务意见。
- 所有 L2 结论均为 simulated L2 或专项 mock 复测，不等同真实 Harness 生产运行通过。
- 平台接入前仍需真实 wrapper smoke test，重点验证输入校验、输出 schema、错误码、日志、人工复核触发和权限边界。
- 默认 `write_actions: none`。任何写入 CRM、发送邮件、发布内容、退款、账号恢复、税务/审计判断，都必须走人工确认或专门合规模块。
- 高风险场景必须触发人工复核，包括投诉、退款、账号安全、隐私信息、财务/税务、医疗/金融/教育合规、低置信度和数据缺失。

## 当前下一步

1. 平台同事对 13 个 Skill 做真实 wrapper smoke test。
2. 下一批 6 个许可证通过候选已完成 L2 simulated 测试：4 个送 L3 封装，2 个保留为需受控真实 Harness 补测。
3. `support_response_eval_suite`、`hr_resume_privacy_masker`、`support_rag_eval_runner`、`contract_section_partitioner` 进入封装专员队列。
4. `quote_pdf_table_extractor` 和 `sales_meeting_transcriber` 暂不封装，等待指挥中心单独确认本地 PDF/短音频补测边界。
5. PaddleOCR / `receipt_line_ocr_extractor` 与 WhisperX / `sales_speaker_timeline` 均为 L2 通过但仅组件定位，不进入独立 L3。
6. 组件池先按组合包方案推进，不作为独立 L3 Skill。
