# P1 扩容正式 Skill 索引

更新时间: 2026-06-06

## 口径

- 本批次按用户确认的新规则从候选库转入稳定库。
- 本批次数量: 42。
- 当前稳定库总数: 55 = P0 首批 13 + P1 扩容 42。
- 这些 Skill 可作为 Agent Skill 安装或迁移，但必须保留各自 `skill.yaml` / `SKILL.md` 中的 mock、read-only、dry-run、禁止发送、禁止写入、禁止上传、禁止真实外部动作等边界。
- 本批次不包含真实媒体生成 provider、许可证/manifest 未补齐的标准 Skill、role/component-only 项。

## Skill 清单

| # | Skill ID | 类型 |
| ---: | --- | --- |
| 1 | `sheets_metrics_report_dryrun_agent` | dry-run / report |
| 2 | `notion_kb_gap_review_dryrun_agent` | dry-run / knowledge |
| 3 | `slack_support_triage_dryrun_agent` | dry-run / support |
| 4 | `hubspot_crm_followup_dryrun_agent` | dry-run / sales |
| 5 | `excel_metrics_report_dryrun_agent` | dry-run / report |
| 6 | `square_pos_daily_report_dryrun_agent` | dry-run / store |
| 7 | `gmail_support_email_triage_dryrun_agent` | dry-run / support |
| 8 | `outlook_followup_draft_dryrun_agent` | dry-run / sales-support |
| 9 | `zoho_crm_followup_dryrun_agent` | dry-run / sales |
| 10 | `pipedrive_deal_next_step_dryrun_agent` | dry-run / sales |
| 11 | `lark_meeting_action_dryrun_agent` | dry-run / collaboration |
| 12 | `wecom_group_summary_dryrun_agent` | dry-run / private-domain |
| 13 | `gorgias_support_ticket_triage_dryrun_agent` | dry-run / ecommerce-support |
| 14 | `zoho_books_expense_reconcile_dryrun_agent` | dry-run / finance |
| 15 | `shopify_catalog_qc_readonly_agent` | read-only / ecommerce |
| 16 | `stripe_subscription_health_readonly_agent` | read-only / finance-renewal |
| 17 | `notion_policy_gap_readonly_agent` | read-only / knowledge |
| 18 | `airtable_inventory_alert_readonly_agent` | read-only / inventory |
| 19 | `slack_faq_gap_readonly_agent` | read-only / support |
| 20 | `google_drive_doc_classifier_readonly_agent` | read-only / docs |
| 21 | `hubspot_contact_health_dryrun_agent` | dry-run / sales |
| 22 | `quickbooks_cashflow_warning_readonly_agent` | read-only / finance |
| 23 | `zendesk_sla_macro_gap_readonly_agent` | read-only / support |
| 24 | `freshdesk_ticket_sla_risk_readonly_agent` | read-only / support |
| 25 | `salesforce_opportunity_hygiene_dryrun_agent` | dry-run / sales |
| 26 | `shopline_catalog_qc_readonly_agent` | read-only / ecommerce |
| 27 | `lightspeed_pos_shift_anomaly_readonly_agent` | read-only / store |
| 28 | `xero_bank_reconcile_exception_readonly_agent` | read-only / finance |
| 29 | `posthog_funnel_dropoff_readonly_agent` | read-only / analytics |
| 30 | `linear_customer_bug_triage_readonly_agent` | read-only / support-product |
| 31 | `monday_board_health_readonly_agent` | read-only / ops |
| 32 | `clickup_task_risk_readonly_agent` | read-only / ops |
| 33 | `intercom_conversation_macro_gap_readonly_agent` | read-only / support |
| 34 | `helpscout_article_gap_readonly_agent` | read-only / support |
| 35 | `front_inbox_handoff_readonly_agent` | read-only / support-sales |
| 36 | `klaviyo_campaign_health_readonly_agent` | read-only / marketing |
| 37 | `woocommerce_catalog_qc_readonly_agent` | read-only / ecommerce |
| 38 | `bigcommerce_catalog_gap_readonly_agent` | read-only / ecommerce |
| 39 | `google_ads_creative_budget_anomaly_readonly_agent` | read-only / marketing-ads |
| 40 | `ga4_landing_page_dropoff_readonly_agent` | read-only / analytics |
| 41 | `canva_design_brief_dryrun_agent` | dry-run / design-brief |
| 42 | `jira_service_sla_readonly_agent` | read-only / service-desk |

## 统一边界

- 不读取或写入 API key、`.env`、真实客户文件。
- 不访问真实账号。
- 不发送邮件、短信、群消息或平台通知。
- 不写 CRM、POS、财务、HR、协作、客服、电商、广告或分析系统。
- 不上传素材，不生成真实媒体，不发布，不退款、不扣款、不改库存、不改订阅、不写账、不报税。
- 后续真实 Harness 只作为增强验收，不改变本批次作为正式 Agent Skill 安装或迁移的状态。
