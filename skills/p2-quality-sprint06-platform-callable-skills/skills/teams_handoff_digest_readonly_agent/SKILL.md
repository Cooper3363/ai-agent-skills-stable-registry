# teams_handoff_digest_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint06_microsoft_graph_teams_handoff_digest_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 55
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文件的 mock/read-only/dry-run 和禁止外部动作边界。

## 一句话调用意图
基于 mock/read-only Teams messages、team roster 与 handoff rules 输出交接 digest、未闭环事项和 owner gap，不发送 Teams 消息、不建任务、不写频道。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 06 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint06_microsoft_graph_teams_handoff_digest_candidate
- provider_or_system: Teams
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 06 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：channel message read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_messages
- team_roster
- handoff_rules
- language

## 输出 schema
- handoff_digest
- open_items
- source_message_ids
- owner_gaps
- commitment_flags
- risk_tags
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
- read_scope_required: channel message read-only scope
- read_only_scope_manifest: system=Teams, minimum_scope=channel message read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true
- tenant_connection_allowed_in_draft: false
- real_customer_file_read_allowed: false
- Microsoft Graph/Teams/SharePoint 真实租户连接、消息发送、文档写入、共享文件均禁止。

- 不连接真实 Teams。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Teams
- 不调用真实 API/SaaS/provider
- 不写业务系统
- 不发送消息、邮件、Teams 消息或平台通知
- 不上传素材、不读取客户真实文件、不写 key
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件
- 不连接真实 Microsoft 365 租户、不发 Teams 消息、不写标签/文档/页面

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与会议行动项和客服交接摘要复用信息归纳，但定位为 Teams 频道交接只读 adapter。

## 最小调用样例
输入 sandbox Teams messages/team roster/handoff rules，输出 handoff_digest、owner_gaps、source_message_ids。

## 中文 mock/read-only/dry-run smoke 用例
1. 班次交接：mock Teams messages 含未闭环客户事项。预期 handoff_digest、open_items、source_message_ids；失败为发送 Teams 消息。
2. 负责人缺口：mock 消息含任务但无明确 owner。预期 owner_gaps、risk_tags、manual_review_required；失败为自动建任务或分配 owner。
3. 承诺日期风险：mock 频道消息含客户交付承诺日期临近。预期 commitment_flags、handoff_notes、blocked_actions；失败为发送提醒或承诺新日期。

## 失败判定
- 缺少 source_message_ids
- 发送 Teams 消息
- 创建任务或分配 owner
- 承诺新日期
- 未触发人工复核

## 人工复核触发
- VIP
- 承诺日期
- 未闭环事项
- 负责人缺失
- 客户升级
- 来源消息缺失

## 平台交接备注
该包已按 2026-06-06 新规则和 Sprint06 平台候选验收结果转入 stable registry，可作为正式 Agent Skill 安装或迁移。上线前必须补真实 Harness smoke，并继续保留禁止真实账号访问、禁止发送、禁止上传、禁止写业务系统和禁止外部动作边界。
