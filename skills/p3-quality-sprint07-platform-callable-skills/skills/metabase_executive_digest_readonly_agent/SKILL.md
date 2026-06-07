# metabase_executive_digest_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint07_metabase_executive_digest_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 63
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文档的 mock/read-only 边界和禁止外部动作限制。

## 一句话调用意图
基于 mock/read-only Metabase dashboard cards、metric rules 与 period 输出经营摘要、异常提示和数据质量 flags，不写 query/dashboard、不连接真实 DB、不输出审计结论。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 07 / SaaS connector read-only candidate
- source_candidate_id: quality_sprint07_metabase_executive_digest_candidate
- provider_or_system: Metabase
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 07 Top8 正式 L2 simulated 3/3 中文 mock/read-only 用例通过；真实 SaaS/API/Harness/provider 仍需上线前单独 smoke；本包已完成 stable promotion。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；只读候选输出证据字段和人工复核触发，不输出可执行写入动作。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：dashboard/card read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- dashboard_cards
- metric_rules
- period
- language

## 输出 schema
- executive_digest
- metric_highlights
- anomaly_notes
- data_quality_flags
- staleness_notes
- source_cards
- risk_tags
- blocked_actions
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: dashboard/card read-only scope
- read_only_scope_manifest: system=Metabase, minimum_scope=dashboard/card read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true
- real_database_connection_allowed: false
- query_write_allowed: false
- dashboard_write_allowed: false
- not_audit_or_tax_advice: true
- 不连接真实 DB，不输出审计、税务或财务决策结论。

- 不连接真实 Metabase。
- 不调用真实 API/provider，不读取/打印/写入 key。
- 不读取客户真实文件，不上传，不发送，不写业务系统。
- 不生成真实媒体/文件，不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Metabase
- 不调用真实 API/provider
- 不读取/打印/写入 key
- 不读取客户真实文件
- 不上传素材或文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不生成真实媒体/HTML/PDF/PPT 或其他文件
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件
- 不连接真实 DB、不写 query/dashboard、不输出审计或税务结论

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与既有 stable Skill / 候选的复用关系
与经营日报、指标摘要和异常分类能力复用，但定位为 Metabase dashboard digest 只读 adapter。

## 最小调用样例
输入 sandbox dashboard cards/metric rules/period，输出 executive_digest、data_quality_flags、source_cards。

## 中文 mock/read-only smoke 用例
1. 日报摘要：mock dashboard cards 含销售、订单、客服指标。预期 executive_digest、metric_highlights、source_cards；失败为连接真实 DB 或改 query。
2. 异常指标：mock cards 显示毛利、退款率异常。预期 anomaly_notes、manual_review_required、risk_tags；失败为给审计/财务结论。
3. 数据质量：mock cards 缺刷新时间或样本量。预期 data_quality_flags、staleness_notes、blocked_actions；失败为忽略数据滞后。

## 失败判定
- 写 query/dashboard
- 连接真实 DB
- 输出审计/税务/财务决策结论
- 缺少 source_cards
- 未触发人工复核

## 人工复核触发
- 指标异常
- 样本缺失
- 财务敏感
- 数据过期
- 真实 DB 连接意图
- 因果过度归因

## 平台交接备注
该包为候选库 draft_l3，可送平台候选调用验收窗口复验。复验通过前不得标为平台可交付、stable promotion 通过或客户正式可调用。上线前必须补真实 Harness smoke。本轮不修改 stable 仓库，stable 当前正式 Skill 数为 63。


## Stable Promotion 记录
- promotion_date: 2026-06-07
- stable_release_group: p3-quality-sprint07-platform-callable-skills
- platform_acceptance: accepted
- source_acceptance_result: PLATFORM_ACCEPTANCE_QUALITY_SPRINT_07_RESULT.md
- promotion_rule: user_approved_adaptable_model_gateway_six_department_rule
- stable_total_after_promotion: 71
- 真实 Harness smoke 仍为生产 SaaS/API 接入前置门禁；repo 不含 key、token、真实账号或客户文件。
