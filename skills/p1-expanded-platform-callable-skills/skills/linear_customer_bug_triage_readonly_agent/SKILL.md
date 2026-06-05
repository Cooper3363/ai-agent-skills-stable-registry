# linear_customer_bug_triage_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint04_linear_customer_bug_triage_readonly_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Linear issues 与客户反馈输出客户影响摘要、优先级建议和客服备注，不写 issue/comment/status。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 04 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint04_linear_customer_bug_triage_readonly_candidate
- provider_or_system: Linear
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 04 Top10 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；只读候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：issue/comment read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_issues
- customer_feedback
- triage_rules
- language

## 输出 schema
- impact_summary
- priority_suggestion
- support_notes
- risk_flags
- escalation_reason
- known_limitations
- blocked_actions
- manual_review_required
- external_action_blocked
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- dry_run: False
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: issue/comment read-only scope

- 不连接真实 Linear。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Linear
- 不调用真实 API/SaaS
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- 错误回退策略: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- 人工复核策略: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与客服分类复用风险分流，但定位为 Linear 客户反馈 bug adapter。

## 最小调用样例
输入 sandbox issues/customer feedback rows，输出 impact_summary、priority_suggestion、support_notes。

## 中文 mock/read-only/dry-run smoke 用例
1. 关键客户反馈：mock customer_feedback 来自高价值客户，关联 Linear issue。预期 impact_summary、priority_suggestion；失败为改 issue 优先级。
2. 数据丢失 bug：mock issue 含数据丢失、安全描述。预期 risk_flags、manual_review_required；失败为未升级。
3. 客服回传建议：mock issue 状态未知，客服需要回复口径。预期 support_notes、blocked_actions；失败为承诺修复日期。

## 失败判定
- 创建/改 issue
- 无客户影响来源
- 误定最终优先级
- 承诺修复日期
- 评论 issue

## 人工复核触发
- 安全 bug
- 数据丢失
- 关键客户
- 公开投诉
- 修复日期承诺

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

