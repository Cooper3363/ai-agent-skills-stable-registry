# P0 首批 13 个 Skill 索引

| # | Skill ID | 中文名 | 业务包 | 状态 | 来源项目 | 许可证 | 一句话用途 |
| ---: | --- | --- | --- | --- | --- | --- | --- |
| 1 | `marketing_copy_pack` | 营销文案包 | 营销内容 | 可交付平台 | Marketing Skills | MIT | 生成标题、正文、CTA 和合规提示，默认只产出草稿 |
| 2 | `daily_weekly_metrics_reporter` | 日报周报生成器 | 经营报表 | 可交付平台 | Instructor | MIT | 根据经营指标生成日报、周报或月报 |
| 3 | `metric_exception_classifier` | 指标异常分类器 | 经营报表 | 可交付平台 | Marvin | Apache-2.0 | 对经营指标异常分级分类并给核查建议 |
| 4 | `faq_answer_with_citations` | 带引用 FAQ 问答 | 客服知识库 | 可交付平台 | Haystack | Apache-2.0 | 根据知识库回答客服问题并返回真实引用 |
| 5 | `support_ticket_classifier` | 售后工单分类器 | 客服知识库 | 可交付平台 | Instructor | MIT | 对售后工单分类、定优先级、建议转人工 |
| 6 | `structured_campaign_brief` | 活动 brief 结构化 | 营销内容 | 可交付平台 | Instructor | MIT | 将活动需求整理成素材需求和投放变量 JSON |
| 7 | `structured_metric_summary` | 单项指标摘要 | 经营报表 | 可交付平台 | Instructor | MIT | 对单项指标做结构化摘要和保守建议 |
| 8 | `crm_note_structurer` | CRM 记录结构化 | 销售跟进 | 可交付平台 | Instructor | MIT | 整理销售记录、客户异议和下一步动作 |
| 9 | `support_reply_guardrail` | 客服回复安全检查 | 客服知识库 | 可交付平台 | Guardrails | Apache-2.0 | 检查退款承诺、账号安全、投诉升级等风险 |
| 10 | `marketing_compliance_guard` | 营销合规检查 | 营销内容 | 可交付平台 | Guardrails | Apache-2.0 | 检查禁词、夸大承诺和敏感行业风险 |
| 11 | `support_pii_redactor` | 中文客服 PII 脱敏 | 客服知识库 | 可交付平台 | Microsoft Presidio | MIT | 脱敏手机号、邮箱、身份证、地址、订单号等 |
| 12 | `support_kb_doc_cleaner` | 知识库文档清洗 | 客服知识库 | 可交付平台 | Unstructured | Apache-2.0 | 清洗客服知识库前置文本并输出 chunks |
| 13 | `expense_invoice_parser` | 费用票据字段抽取 | 经营报表/财票 | 可交付平台 | invoice2data | MIT | 从本地/mock 票据文本抽取结构化字段 |

## 状态说明

- 可交付平台：已通过平台调用验收，可进入平台接入队列，但上线前仍需真实 wrapper smoke test。
- 不在本表：组件池、暂缓观察项、需法务/安全复核项。
