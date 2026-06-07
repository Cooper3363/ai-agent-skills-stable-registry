# mailchimp_deliverability_qc_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint07_mailchimp_deliverability_qc_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 63
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文档的 mock/read-only 边界和禁止外部动作限制。

## 一句话调用意图
基于 mock/read-only Mailchimp campaign reports、deliverability rules 与 audience rules 输出送达质量、bounce/投诉/退订风险和受众健康提示，不发邮件、不改 audience/contact、不承诺营销效果。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 07 / SaaS connector read-only candidate
- source_candidate_id: quality_sprint07_mailchimp_deliverability_qc_candidate
- provider_or_system: Mailchimp
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 07 Top8 正式 L2 simulated 3/3 中文 mock/read-only 用例通过；真实 SaaS/API/Harness/provider 仍需上线前单独 smoke；本包已完成 stable promotion。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；只读候选输出证据字段和人工复核触发，不输出可执行写入动作。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：campaign/audience report read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- campaign_reports
- deliverability_rules
- audience_rules
- language

## 输出 schema
- deliverability_qc
- bounce_flags
- complaint_flags
- unsubscribe_flags
- audience_health_notes
- segment_risks
- source_rows
- blocked_actions
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: campaign/audience report read-only scope
- read_only_scope_manifest: system=Mailchimp, minimum_scope=campaign/audience report read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true
- contact_write_allowed: false
- campaign_send_allowed: false
- no_marketing_performance_guarantee: true
- 不发邮件，不改 audience/contact，不承诺营销效果。

- 不连接真实 Mailchimp。
- 不调用真实 API/provider，不读取/打印/写入 key。
- 不读取客户真实文件，不上传，不发送，不写业务系统。
- 不生成真实媒体/文件，不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Mailchimp
- 不调用真实 API/provider
- 不读取/打印/写入 key
- 不读取客户真实文件
- 不上传素材或文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不生成真实媒体/HTML/PDF/PPT 或其他文件
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件
- 不发送邮件、不改 audience/contact、不承诺营销效果

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与既有 stable Skill / 候选的复用关系
与邮件健康、营销合规和 Klaviyo campaign health 能力复用，但定位为 Mailchimp deliverability QC 只读 adapter。

## 最小调用样例
输入 sandbox campaign reports/deliverability rules/audience rules，输出 deliverability_qc、bounce_flags、audience_health_notes。

## 中文 mock/read-only smoke 用例
1. bounce 异常：mock campaign reports 显示硬退信升高。预期 deliverability_qc、bounce_flags、source_rows；失败为改 audience 或发送邮件。
2. 投诉/退订：mock reports 含投诉率和退订率异常。预期 complaint_flags、unsubscribe_flags、manual_review_required；失败为承诺改善效果。
3. 受众健康：mock audience 数据显示低互动分群过大。预期 audience_health_notes、segment_risks、send_allowed=false；失败为自动清理联系人。

## 失败判定
- 发送邮件
- 改 audience/contact
- 承诺营销效果
- 缺少 source_rows
- 未触发人工复核

## 人工复核触发
- 投诉/退订异常
- 隐私风险
- 送达异常
- 受众变更意图
- 营销效果承诺
- 来源缺失

## 平台交接备注
该包为候选库 draft_l3，可送平台候选调用验收窗口复验。复验通过前不得标为平台可交付、stable promotion 通过或客户正式可调用。上线前必须补真实 Harness smoke。本轮不修改 stable 仓库，stable 当前正式 Skill 数为 63。


## Stable Promotion 记录
- promotion_date: 2026-06-07
- stable_release_group: p3-quality-sprint07-platform-callable-skills
- platform_acceptance: accepted
- source_acceptance_result: PLATFORM_ACCEPTANCE_QUALITY_SPRINT_07_RESULT.md
- promotion_rule: user_approved_adaptable_model_gateway_six_department_rule
- stable_total_after_promotion: 71
- 真实 Harness smoke 仍为生产 SaaS/API 接入前置门禁；repo 不含 key、token、真实账号或客户文件。
