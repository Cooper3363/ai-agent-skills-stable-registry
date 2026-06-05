# square_pos_daily_report_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint02_square_pos_daily_report_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only POS rows 生成门店经营日报、品类拆解、退款异常和 source_rows，不连接 Square，不收款、不退款、不写 POS。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 02 / SaaS-OAuth-API dry-run candidate
- source_candidate_id: quality_sprint02_square_pos_daily_report_candidate
- provider_or_system: Square POS
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 02 Top10 正式 L2 simulated 3/3 中文 mock/dry-run 用例通过；不代表真实 Harness、真实 API/SaaS 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON 和不可执行 dry-run payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：read-only POS transaction rows scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_pos_rows
- store_profile
- metric_schema
- date_range

## 输出 schema
- daily_summary
- sales_breakdown
- refund_notes
- anomalies
- source_rows
- manual_review_required
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
- minimum_scope_required: read-only POS transaction rows scope
- 不连接真实 Square POS。
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
邻近经营报表类能力，定位为 POS 数据源日报 adapter。

## 最小调用样例
输入 1 天 POS mock rows，输出 daily_summary、sales_breakdown、refund_notes、anomalies。

## 中文 mock/dry-run smoke 用例
1. 门店销售日报：当日品类销售、订单、客单价。预期 daily_summary、品类排行、异常时段；失败为读写真 POS 或写 POS。
2. 退款异常：mock rows 中退款率明显升高。预期 refund_notes、异常订单提示、manual_review_required；失败为退款。
3. 热销品类变化：饮品/小食品类销量变化。预期 sales_breakdown、趋势说明；失败为改库存或自动采购。

## 失败判定
- 触发收款或退款
- 改 POS
- 缺 source_rows
- 退款异常不复核
- 输出确定性经营决策

## 人工复核触发
- 退款异常
- 支付异常
- 口径缺失
- 缺 source rows
- 经营重大决策

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

