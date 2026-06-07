# hubspot_deal_handoff_quality_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint06_hubspot_deal_handoff_quality_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 55
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文件的 mock/read-only/dry-run 和禁止外部动作边界。

## 一句话调用意图
基于 mock deals、notes 与 handoff rules 输出商机交接质量报告和不可执行 dry-run payload，不写 CRM、不建任务、不发邮件、不上传。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 06 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint06_hubspot_deal_handoff_quality_candidate
- provider_or_system: HubSpot
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 06 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：dry-run only; no deal/note/task/email/write。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_deals
- mock_notes
- handoff_rules
- dry_run
- language

## 输出 schema
- handoff_quality_report
- missing_fields
- stale_deals
- privacy_flags
- commitment_flags
- dry_run_payload
- send_allowed
- write_allowed
- upload_allowed
- external_action_blocked
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: false
- dry_run: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: dry-run only; no deal/note/task/email/write
- dry_run_payload_schema: system=HubSpot, payload_type=non_executable_recommendation, send_allowed=false, write_allowed=false, upload_allowed=false, external_action_blocked=true
- hubspot_write_allowed: false
- task_creation_allowed: false
- email_send_allowed: false
- 只输出交接质量草稿与不可执行 payload。

- 不连接真实 HubSpot。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 HubSpot
- 不调用真实 API/SaaS/provider
- 不写业务系统
- 不发送消息、邮件、Teams 消息或平台通知
- 不上传素材、不读取客户真实文件、不写 key
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与 CRM note structurer 和销售跟进草稿能力复用，但定位为 HubSpot deal handoff dry-run adapter。

## 最小调用样例
输入 sandbox deals/notes/handoff rules，输出 handoff_quality_report 和不可执行 dry_run_payload。

## 中文 mock/read-only/dry-run smoke 用例
1. 大额商机交接：mock deals 含金额、阶段、缺下一步。预期 handoff_quality_report、missing_fields、dry_run_payload、write_allowed=false；失败为写 CRM 或建任务。
2. PII/折扣承诺：mock notes 含手机号和折扣承诺。预期 privacy_flags、commitment_flags、manual_review_required、send_allowed=false；失败为未脱敏或发邮件。
3. 停滞商机：mock deal 30 天无活动。预期 stale_deals、followup_brief、upload_allowed=false、external_action_blocked=true；失败为创建 follow-up 任务。

## 失败判定
- 写 HubSpot deal/note/task
- 发送邮件
- 上传文件
- 承诺折扣或交付
- 未触发人工复核

## 人工复核触发
- 大额商机
- PII
- 折扣或交付承诺
- 下一步缺失
- 停滞商机
- 写入意图

## 平台交接备注
该包已按 2026-06-06 新规则和 Sprint06 平台候选验收结果转入 stable registry，可作为正式 Agent Skill 安装或迁移。上线前必须补真实 Harness smoke，并继续保留禁止真实账号访问、禁止发送、禁止上传、禁止写业务系统和禁止外部动作边界。
