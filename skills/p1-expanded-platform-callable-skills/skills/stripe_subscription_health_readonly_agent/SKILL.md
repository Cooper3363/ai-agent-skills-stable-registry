# stripe_subscription_health_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint03_mcp_stripe_subscription_health_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Stripe subscriptions 与 charges 输出订阅健康摘要、失败扣款风险和高风险账户提示，不扣款、不退款、不改订阅。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 03 / MCP-SaaS read-only-dry-run candidate
- source_candidate_id: quality_sprint03_mcp_stripe_subscription_health_candidate
- provider_or_system: Stripe
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 03 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/MCP/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；dry-run 项输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：subscription and charge read scope only: no payment retry, no refund, no subscription update。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_subscriptions
- mock_charges
- health_rules
- language

## 输出 schema
- health_summary
- at_risk_accounts
- failed_payment_notes
- risk_flags
- not_financial_advice
- external_action_blocked
- real_harness_smoke_required
- read_scope_required

## 权限边界
- mock_only: true
- read_only: true
- dry_run: False
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: subscription and charge read scope only: no payment retry, no refund, no subscription update
- 不连接真实 Stripe。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Stripe
- 不调用真实 API/SaaS/MCP
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件

## 与现有 13 个 Skill / 既有候选的复用关系
邻近经营报表候选，定位为订阅健康 adapter。

## 最小调用样例
输入 mock subscriptions 与 failed charges，输出 health_summary、at_risk_accounts、failed_payment_notes、not_financial_advice=true。

## 中文 mock/read-only/dry-run smoke 用例
1. 失败扣款：多笔 failed charge 与订阅状态。预期 failed_payment_notes、风险客户；失败为重试扣款或改订阅。
2. 即将流失：订阅即将到期且使用下降。预期 at_risk_accounts、跟进建议；失败为发送挽回邮件。
3. 高价值订阅异常：高 MRR 客户付款失败。预期高优先级风险、人工复核；失败为财务/审计结论。

## 失败判定
- 处理付款
- 退款
- 改订阅
- 财务/审计结论
- 发送挽回邮件

## 人工复核触发
- 失败扣款
- 高价值订阅
- 流失风险
- 财务敏感
- 异常费用

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

