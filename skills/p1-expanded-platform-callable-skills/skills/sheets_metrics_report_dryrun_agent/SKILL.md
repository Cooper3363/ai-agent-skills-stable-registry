# sheets_metrics_report_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint01_google_sheets_mcp_report_agent_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Sheets rows 生成经营摘要、指标变化、异常说明和可追溯 source_rows，不连接真实 Google Sheets，不写表。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 01 / MCP-SaaS dry-run candidate
- source_candidate_id: quality_sprint01_google_sheets_mcp_report_agent_candidate
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
- mock_sheet_rows
- metric_schema
- date_range
- language

## 输出 schema
- summary
- metric_changes
- anomalies
- source_rows
- manual_review_required
- risk_flags
- send_allowed
- write_allowed
- external_action_blocked

## 权限边界
- dry_run: true
- read_only: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- dry-run/read-only rows
- 不连接真实 Google Sheets
- 不读取真实客户表格
- 不写表
- 上线前真实 Harness 仅允许授权 read-only scope
- send_allowed=false
- write_allowed=false
- external_action_blocked=true
- 不连接真实 Sheets/Notion/Slack/HubSpot。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。

## 禁止动作
- 不连接真实 Google Sheets
- 不写表
- 不读取客户真实文件
- 不调用真实 API
- 不写 key
- 不输出无法追溯的数据结论

## 与现有 13 个 Skill 的复用关系
邻近 daily_weekly_metrics_reporter / structured_metric_summary，但本包聚焦 Sheets rows 接入前的 dry-run 报表解析和 source_rows 可追溯。

## 最小调用样例
输入 7 行门店销售 mock rows、指标 schema 和日期范围，输出周报摘要、销售环比、异常日期和 source_rows。

## 中文 mock/dry-run smoke 用例
1. 门店日销周报：7 天门店销售、客单价、订单数 mock rows。预期输出 summary、metric_changes、anomalies、source_rows；失败为编造未给出的门店数据。
2. 渠道转化下滑：广告渠道曝光、点击、下单 rows，某渠道 CVR 下滑。预期输出下滑原因假设和需复核项；失败为输出投放承诺或低样本不复核。
3. 库存周转异常：SKU 库存、销量、补货周期 mock rows。预期输出高库存/断货风险和字段缺失提示；失败为改库存或无 source_rows。

## 失败判定
- 编造未给出的数据
- 缺 source_rows
- 异常无解释
- 字段缺失不回退
- 输出投放或经营承诺
- write_allowed 非 false

## 人工复核触发
- 口径冲突
- 异常波动
- 缺关键字段
- source_rows 不足
- 低样本量
- 经营重大决策

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

