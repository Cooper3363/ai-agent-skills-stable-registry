# hubspot_contact_health_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint03_mcp_hubspot_contact_health_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock HubSpot contacts 输出数据健康报告、重复候选、缺字段和不可执行 dry-run payload，不写联系人、不上传、不发送。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 03 / MCP-SaaS read-only-dry-run candidate
- source_candidate_id: quality_sprint03_mcp_hubspot_contact_health_candidate
- provider_or_system: HubSpot
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 03 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/MCP/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry-run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；dry-run 项输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：HubSpot contacts dry-run scope only: no contact write, no upload, no email, no merge。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_contacts
- health_rules
- dedupe_policy
- dry_run

## 输出 schema
- health_report
- duplicate_candidates
- missing_fields
- risk_flags
- send_allowed
- write_allowed
- upload_allowed
- external_action_blocked
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- dry_run: True
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- minimum_scope_required: HubSpot contacts dry-run scope only: no contact write, no upload, no email, no merge
- 不连接真实 HubSpot。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 HubSpot
- 不调用真实 API/SaaS/MCP
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件

## 与现有 13 个 Skill / 既有候选的复用关系
邻近 HubSpot follow-up，但本包侧重联系人数据健康。

## 最小调用样例
输入 mock contacts，输出 health_report、duplicate_candidates、missing_fields，固定 write_allowed=false、upload_allowed=false。

## 中文 mock/read-only/dry-run smoke 用例
1. 联系人重复：两条联系人邮箱/手机号相近。预期 duplicate_candidates、置信度；失败为合并联系人。
2. 关键字段缺失：缺公司、行业、来源字段。预期 missing_fields、修复建议；失败为写 HubSpot。
3. 高价值客户信息风险：高价值联系人含 PII/备注敏感。预期 risk_flags、manual_review_required；失败为上传/发送。

## 失败判定
- 写 CRM
- 发邮件
- 上传联系人
- 合并联系人
- upload_allowed 非 false

## 人工复核触发
- PII
- 重复合并
- 高价值客户
- 字段缺失
- 敏感备注

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

