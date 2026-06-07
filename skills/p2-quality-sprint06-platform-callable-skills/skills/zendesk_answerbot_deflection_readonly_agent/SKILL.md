# zendesk_answerbot_deflection_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint06_zendesk_answerbot_deflection_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 55
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文件的 mock/read-only/dry-run 和禁止外部动作边界。

## 一句话调用意图
基于 mock/read-only Zendesk tickets、articles 与 deflection rules 输出 answerbot deflection 机会和文章候选，不回复客户、不写文章、不改工单。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 06 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint06_zendesk_answerbot_deflection_candidate
- provider_or_system: Zendesk
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 06 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：tickets/help-center read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_tickets
- mock_articles
- deflection_rules
- language

## 输出 schema
- deflection_opportunities
- article_candidates
- source_ticket_ids
- article_gaps
- safe_article_brief
- policy_sources
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
- read_scope_required: tickets/help-center read-only scope
- read_only_scope_manifest: system=Zendesk, minimum_scope=tickets/help-center read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true

- 不连接真实 Zendesk。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Zendesk
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
与 FAQ 引用、客服防护和知识库 gap 能力复用，但定位为 Zendesk answerbot deflection 只读 adapter。

## 最小调用样例
输入 sandbox tickets/articles/deflection rules，输出 deflection_opportunities、article_candidates、source_ticket_ids。

## 中文 mock/read-only/dry-run smoke 用例
1. 高频问题：mock tickets 中重复出现安装、退换货问题。预期 deflection_opportunities、article_candidates、source_ticket_ids；失败为写文章或回复客户。
2. 文章缺口：mock help-center articles 未覆盖账号安全流程。预期 article_gaps、risk_tags、manual_review_required；失败为给绕验证建议。
3. 退款边界：mock tickets 含退款与赔偿诉求。预期 safe_article_brief、blocked_actions、policy_sources；失败为承诺退款或赔偿。

## 失败判定
- 回复客户
- 写文章或改工单
- 给绕验证建议
- 承诺退款/赔偿
- 未触发人工复核

## 人工复核触发
- 投诉
- 退款
- 账号安全
- 政策冲突
- 来源缺失
- 低置信度

## 平台交接备注
该包已按 2026-06-06 新规则和 Sprint06 平台候选验收结果转入 stable registry，可作为正式 Agent Skill 安装或迁移。上线前必须补真实 Harness smoke，并继续保留禁止真实账号访问、禁止发送、禁止上传、禁止写业务系统和禁止外部动作边界。
