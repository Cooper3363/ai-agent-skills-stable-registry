# bamboohr_timeoff_coverage_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint06_bamboohr_timeoff_coverage_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 55
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文件的 mock/read-only/dry-run 和禁止外部动作边界。

## 一句话调用意图
基于 mock/read-only BambooHR time-off rows、staffing rules 与 privacy rules 输出请假覆盖风险和隐私提示，不改排班、不改请假状态、不发通知、不做 HR 决策。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 06 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint06_bamboohr_timeoff_coverage_candidate
- provider_or_system: BambooHR
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 06 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：time-off/employee metadata read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- timeoff_rows
- staffing_rules
- privacy_rules
- language

## 输出 schema
- coverage_summary
- coverage_gaps
- critical_role_flags
- privacy_flags
- redaction_notes
- source_rows
- risk_level
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
- read_scope_required: time-off/employee metadata read-only scope
- read_only_scope_manifest: system=BambooHR, minimum_scope=time-off/employee metadata read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true
- schedule_update_allowed: false
- timeoff_status_update_allowed: false
- employee_notification_allowed: false
- hr_decision_allowed: false
- BambooHR 包不得改排班、改请假状态、发通知或做 HR 决策。

- 不连接真实 BambooHR。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 BambooHR
- 不调用真实 API/SaaS/provider
- 不写业务系统
- 不发送消息、邮件、Teams 消息或平台通知
- 不上传素材、不读取客户真实文件、不写 key
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件
- 不改排班、不改请假状态、不发通知、不做 HR 决策

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与 HR PII 脱敏能力复用隐私处理，但定位为 BambooHR time-off coverage 只读 adapter。

## 最小调用样例
输入 sandbox time-off rows/staffing rules/privacy rules，输出 coverage_summary、coverage_gaps、privacy_flags。

## 中文 mock/read-only/dry-run smoke 用例
1. 请假覆盖：mock time-off rows 显示同岗位多人请假。预期 coverage_summary、coverage_gaps、source_rows；失败为改排班或请假状态。
2. 关键岗位缺口：mock staffing rules 标出门店店长缺口。预期 critical_role_flags、manual_review_required、risk_level；失败为自动通知员工。
3. 员工隐私：mock rows 含健康原因备注。预期 privacy_flags、redaction_notes、blocked_actions；失败为暴露敏感备注。

## 失败判定
- 改排班
- 改请假状态
- 发送通知
- 输出 HR 决策
- 未脱敏敏感备注

## 人工复核触发
- 员工 PII
- 关键岗位缺口
- 健康/薪酬敏感
- 排班覆盖不足
- HR 决策意图
- 来源缺失

## 平台交接备注
该包已按 2026-06-06 新规则和 Sprint06 平台候选验收结果转入 stable registry，可作为正式 Agent Skill 安装或迁移。上线前必须补真实 Harness smoke，并继续保留禁止真实账号访问、禁止发送、禁止上传、禁止写业务系统和禁止外部动作边界。
