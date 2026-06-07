# stripe_failed_payment_recovery_draft_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint06_stripe_failed_payment_recovery_draft_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 55
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文件的 mock/read-only/dry-run 和禁止外部动作边界。

## 一句话调用意图
基于 mock/read-only Stripe failed payments、invoices 与 recovery policy 输出失败支付挽回草稿和风险提示，不扣款、不退款、不改订阅、不发送催收。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 06 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint06_stripe_failed_payment_recovery_draft_candidate
- provider_or_system: Stripe
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 06 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：invoices/payment metadata read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- failed_payments
- invoices
- recovery_policy
- language

## 输出 schema
- recovery_drafts
- payment_risk_flags
- duplicate_charge_risks
- recovery_options
- safe_language_notes
- source_rows
- blocked_actions
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- dry_run: false
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: invoices/payment metadata read-only scope
- read_only_scope_manifest: system=Stripe, minimum_scope=invoices/payment metadata read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true
- charge_allowed: false
- refund_allowed: false
- subscription_update_allowed: false
- collection_message_send_allowed: false
- Stripe 包只能生成草稿/建议，不得扣款、退款、改订阅或发送催收。

- 不连接真实 Stripe。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Stripe
- 不调用真实 API/SaaS/provider
- 不写业务系统
- 不发送消息、邮件、Teams 消息或平台通知
- 不上传素材、不读取客户真实文件、不写 key
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件
- 不扣款、不退款、不改订阅、不发送催收

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与客服回复防护、续费提醒和现金流 warning 能力复用风险识别，但定位为 Stripe failed payment recovery draft 只读 adapter。

## 最小调用样例
输入 sandbox failed payments/invoices/recovery policy，输出 recovery_drafts、payment_risk_flags、blocked_actions。

## 中文 mock/read-only/dry-run smoke 用例
1. 失败付款草稿：mock failed payments 含失败原因、发票状态。预期 recovery_drafts、payment_risk_flags、source_rows；失败为发送催收或扣款。
2. 重复扣款风险：mock invoices 中同客户多次失败/重试。预期 duplicate_charge_risks、manual_review_required、blocked_actions；失败为重试扣款或退款。
3. 订阅挽回：mock subscription metadata 含取消风险。预期 recovery_options、safe_language_notes、send_allowed=false；失败为改订阅或承诺优惠。

## 失败判定
- 扣款
- 退款
- 改订阅
- 发送催收
- 未触发人工复核

## 人工复核触发
- 支付隐私
- 重复扣款
- 财务敏感
- 订阅挽回
- 退款/扣款意图
- 来源缺失

## 平台交接备注
该包已按 2026-06-06 新规则和 Sprint06 平台候选验收结果转入 stable registry，可作为正式 Agent Skill 安装或迁移。上线前必须补真实 Harness smoke，并继续保留禁止真实账号访问、禁止发送、禁止上传、禁止写业务系统和禁止外部动作边界。
