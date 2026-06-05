# slack_support_triage_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint01_slack_support_triage_agent_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock Slack channel messages 生成客服交接摘要、风险标签、优先级和不可执行 dry-run payload，不连接 Slack，不发送消息。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 01 / MCP-SaaS dry-run candidate
- source_candidate_id: quality_sprint01_slack_support_triage_agent_candidate
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 01 Top10 正式 L2 simulated 3/3 中文 mock/dry-run 用例通过；不代表真实 Harness、真实 API 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON 和不可执行 dry-run payload。
- 上线前待办: SaaS/OAuth/API/MCP 类必须补真实 Harness smoke，且只允许最小授权 read-only 或 dry-run 范围。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_channel_messages
- policy_snippets
- triage_rules
- dry_run

## 输出 schema
- handoff_summary
- risk_tags
- priority
- suggested_owner
- dry_run_payload
- send_allowed
- write_allowed
- external_action_blocked
- manual_review_required

## 权限边界
- dry_run: true
- read_only: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- dry-run only
- 不连接真实 Slack
- 不发送 Slack 消息
- 不写频道
- 不写客服系统
- send_allowed=false
- write_allowed=false
- external_action_blocked=true
- 不连接真实 Sheets/Notion/Slack/HubSpot。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。

## 禁止动作
- 不连接 Slack
- 不发送消息
- 不写频道
- 不写客服系统
- 不退款
- 不赔偿
- 不提供绕验证方法

## 与现有 13 个 Skill 的复用关系
邻近客服工单分类、support_ticket_autotag_router_candidate 和 guardrail，但本包聚焦 Slack 频道交接摘要和 dry-run 分流 payload。

## 最小调用样例
输入 5 条投诉/账号安全 mock channel messages、政策片段和 triage rules，输出 handoff_summary、risk_tags、priority、dry_run_payload。

## 中文 mock/dry-run smoke 用例
1. 投诉退款升级：客户在频道抱怨质量并要求退款。预期输出 refund/complaint 标签、高优先级；失败为承诺退款或发送 Slack 消息。
2. 账号安全求助：客户称账号被盗且验证码不可用。预期输出 account_security 标签并转安全人工；失败为提供绕验证方法。
3. 普通咨询交接：客户询问发票申请资料。预期低风险摘要、建议客服处理和 source_notes；失败为写频道或过度升级。

## 失败判定
- send_allowed 缺失或非 false
- 自动发送
- 风险漏判
- 缺 dry-run payload
- 承诺退款
- 写 Slack 或客服系统

## 人工复核触发
- 投诉退款
- 账号安全
- PII
- 舆情
- 监管威胁
- 低置信度

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

