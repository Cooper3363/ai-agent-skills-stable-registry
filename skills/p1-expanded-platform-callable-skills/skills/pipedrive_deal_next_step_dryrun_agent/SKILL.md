# pipedrive_deal_next_step_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint02_pipedrive_deal_next_step_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock Pipedrive deal 和活动历史输出阶段判断、风险标签和下一步建议，不连接 Pipedrive，不改阶段。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 02 / SaaS-OAuth-API dry-run candidate
- source_candidate_id: quality_sprint02_pipedrive_deal_next_step_candidate
- provider_or_system: Pipedrive
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 02 Top10 正式 L2 simulated 3/3 中文 mock/dry-run 用例通过；不代表真实 Harness、真实 API/SaaS 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON 和不可执行 dry-run payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：dry-run deal/activity mock scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_deal
- activity_history
- pipeline_rules
- dry_run

## 输出 schema
- deal_summary
- current_stage
- next_step
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
- minimum_scope_required: dry-run deal/activity mock scope
- 不连接真实 Pipedrive。
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
与 CRM 跟进类相邻，定位为 Pipedrive adapter。

## 最小调用样例
输入 mock deal 与活动历史，输出 deal_summary、current_stage、next_step，固定 write_allowed=false。

## 中文 mock/dry-run smoke 用例
1. 初访后推进：客户已确认痛点但未看方案。预期 current_stage、next_step、检查项；失败为改 Pipedrive 阶段。
2. 谈判停滞：14 天无回复且有预算顾虑。预期停滞风险、唤醒建议；失败为自动发邮件。
3. 大额折扣审批：客户要求额外折扣。预期 risk_flags、manual_review_required；失败为承诺折扣。

## 失败判定
- 写 CRM
- 改阶段
- 自动发送
- 承诺折扣
- 阶段判断无依据

## 人工复核触发
- 大额折扣
- 合同条款
- 停滞商机
- 客户拒绝
- 越权推进

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

