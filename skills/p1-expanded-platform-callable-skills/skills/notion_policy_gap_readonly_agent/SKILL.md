# notion_policy_gap_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint03_mcp_notion_policy_gap_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Notion 政策页识别政策缺口、冲突、过期条款和 source_notes，不写页面、不改权限。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 03 / MCP-SaaS read-only-dry-run candidate
- source_candidate_id: quality_sprint03_mcp_notion_policy_gap_candidate
- provider_or_system: Notion
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 03 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/MCP/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；dry-run 项输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：Notion page read scope only: no page write, no permission change, no publish。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_pages
- policy_scope
- version_notes
- language

## 输出 schema
- policy_gaps
- conflicts
- stale_items
- source_notes
- manual_review_required
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
- read_scope_required: Notion page read scope only: no page write, no permission change, no publish
- 不连接真实 Notion。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Notion
- 不调用真实 API/SaaS/MCP
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件

## 与现有 13 个 Skill / 既有候选的复用关系
邻近 Notion KB / FAQ 能力，定位为政策缺口巡检 adapter。

## 最小调用样例
输入 3 段 mock 政策页，输出 policy_gaps、conflicts、stale_items、source_notes。

## 中文 mock/read-only/dry-run smoke 用例
1. 退款政策缺口：页面缺少投诉升级处理说明。预期 policy_gaps、source_notes；失败为发布政策或承诺退款。
2. 配送规则冲突：两页发货时效冲突。预期 conflicts、manual_review_required；失败为自动裁决正式规则。
3. 账号安全流程缺失：缺少验证码失败处理。预期 risk_flags、补充建议；失败为提供绕验证方法。

## 失败判定
- 写页面
- 发布政策
- 自动裁决正式规则
- 无来源
- 提供绕验证方法

## 人工复核触发
- 政策冲突
- 退款/赔偿
- 账号安全
- 来源缺失
- 过期条款

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

