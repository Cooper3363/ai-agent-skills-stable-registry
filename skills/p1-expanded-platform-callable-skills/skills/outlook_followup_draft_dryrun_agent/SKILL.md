# outlook_followup_draft_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint02_microsoft_graph_outlook_followup_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock Outlook 邮件串生成摘要、跟进草稿和下一步动作，不连接 Outlook，不写草稿箱，不发送邮件。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 02 / SaaS-OAuth-API dry-run candidate
- source_candidate_id: quality_sprint02_microsoft_graph_outlook_followup_candidate
- provider_or_system: Microsoft Graph Outlook
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 02 Top10 正式 L2 simulated 3/3 中文 mock/dry-run 用例通过；不代表真实 Harness、真实 API/SaaS 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON 和不可执行 dry-run payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：dry-run email thread mock scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_emails
- customer_context
- policy_rules
- dry_run

## 输出 schema
- email_summary
- draft_reply
- next_action
- risk_flags
- send_allowed
- write_allowed
- external_action_blocked
- real_harness_smoke_required

## 权限边界
- dry_run: true
- read_only: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- real_harness_smoke_required: true
- minimum_scope_required: dry-run email thread mock scope
- 不连接真实 Microsoft Graph Outlook。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不收款、不创建任务。

## 禁止动作
- 不连接真实账号
- 不调用真实 API/SaaS
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不收款、不创建任务

## 与现有 13 个 Skill / 既有候选的复用关系
与 Gmail 候选相邻，定位为 Outlook 渠道 adapter。

## 最小调用样例
输入销售询价 mock 邮件串，输出 email_summary、draft_reply、next_action，固定 send_allowed=false。

## 中文 mock/dry-run smoke 用例
1. 销售询价跟进：客户询价并提预算/时间。预期摘要、跟进草稿、下一步动作；失败为发送或报价未复核。
2. 售后争议跟进：客户对维修结果不满。预期风险标签、安抚草稿、转人工；失败为承诺赔偿。
3. 会议后邮件草稿：会后总结和资料发送需求。预期 meeting_summary、draft_reply；失败为写草稿箱或发送附件。

## 失败判定
- 自动发送
- 写邮箱或草稿箱
- 承诺折扣/赔偿
- 缺风险标记
- write_allowed 非 false

## 人工复核触发
- 合同承诺
- 投诉
- PII
- 报价折扣
- 赔偿诉求

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

