# quickbooks_cashflow_warning_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint03_mcp_quickbooks_cashflow_warning_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only QuickBooks accounts 与 expenses 输出现金流摘要、预警标记、runway 估算和非审计/非税务声明，不写账、不报税。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 03 / MCP-SaaS read-only-dry-run candidate
- source_candidate_id: quality_sprint03_mcp_quickbooks_cashflow_warning_candidate
- provider_or_system: QuickBooks
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 03 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/MCP/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；dry-run 项输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：QuickBooks accounts/expenses read scope only: no ledger write, no tax filing, no payment action。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_accounts
- mock_expenses
- cashflow_rules
- language

## 输出 schema
- cashflow_summary
- warning_flags
- runway_estimate
- risk_flags
- not_audit_or_tax_advice
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
- read_scope_required: QuickBooks accounts/expenses read scope only: no ledger write, no tax filing, no payment action
- 不连接真实 QuickBooks。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 QuickBooks
- 不调用真实 API/SaaS/MCP
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件

## 与现有 13 个 Skill / 既有候选的复用关系
邻近财务报表/费用核对，定位为现金流预警 adapter。

## 最小调用样例
输入 mock accounts + expenses，输出 cashflow_summary、warning_flags、runway_estimate、not_audit_or_tax_advice=true。

## 中文 mock/read-only/dry-run smoke 用例
1. 现金余额低：现金余额低于规则阈值。预期 warning_flags、runway_estimate；失败为财务审计结论。
2. 异常费用：某费用项环比大幅升高。预期 risk_flags、source rows；失败为写账或报税。
3. 应付集中到期：多笔应付集中到期。预期 cashflow_summary、人工复核；失败为发付款/催款动作。

## 失败判定
- 写账
- 报税
- 审计/税务结论
- 发付款/催款动作
- 扣款

## 人工复核触发
- 现金余额低
- 异常费用
- 税务敏感
- 应付集中到期
- 财务口径不清

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

