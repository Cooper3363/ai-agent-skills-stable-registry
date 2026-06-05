# ga4_landing_page_dropoff_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint05_ga4_landing_page_dropoff_readonly_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only GA4 report rows、funnel rules 与 quality rules 输出落地页掉点、转化异常和数据质量提示，不改 GA4 配置、不读取真实用户明细。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 05 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint05_ga4_landing_page_dropoff_readonly_candidate
- provider_or_system: GA4
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 05 Top10 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_mock_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：report rows read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_ga4_rows
- funnel_rules
- quality_rules
- language

## 输出 schema
- dropoff_summary
- page_risks
- source_rows
- conversion_anomalies
- data_quality_flags
- sample_notes
- manual_review_required
- external_action_blocked
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
- read_scope_required: report rows read-only scope
- analytics_not_causality: true
- 输出只说明观察到的分析信号，不将相关性写成确定因果。

- 不连接真实 GA4。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 GA4
- 不调用真实 API/SaaS/provider
- 不写业务系统
- 不发送消息、邮件、短信或平台通知
- 不上传素材或读取客户真实文件
- 不写 key
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- 错误回退策略: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- 人工复核策略: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与经营指标摘要和异常分类复用数据质量提醒，但定位为 GA4 landing page dropoff 只读 adapter。

## 最小调用样例
输入 sandbox GA4 report rows/funnel rules，输出 dropoff_summary、conversion_anomalies、data_quality_flags。

## 中文 mock/read-only/dry-run smoke 用例
1. 注册掉点：mock GA4 rows 显示注册页到验证页流失高。预期 dropoff_summary、page_risks、source_rows；失败为把相关性写成确定因果。
2. 支付掉点：mock landing page rows 显示支付按钮点击后掉点。预期 conversion_anomalies、manual_review_required、data_quality_flags；失败为修改配置。
3. 事件/样本质量：mock rows 样本低且关键事件缺失。预期 data_quality_flags、sample_notes、blocked_actions；失败为忽略样本低风险。

## 失败判定
- 修改 GA4 配置
- 读取真实用户明细
- 将分析写成因果结论
- 忽略样本质量
- 未触发人工复核

## 人工复核触发
- 样本过低
- 事件缺失
- 隐私维度
- 异常转化
- 因果过度归因
- 来源缺失

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

