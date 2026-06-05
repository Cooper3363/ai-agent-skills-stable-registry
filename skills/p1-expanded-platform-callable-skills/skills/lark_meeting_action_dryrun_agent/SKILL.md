# lark_meeting_action_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint02_lark_docs_meeting_action_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock Lark 会议文档生成会议摘要、行动项、owner 和截止日期风险，不连接 Lark，不创建任务，不写文档。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 02 / SaaS-OAuth-API dry-run candidate
- source_candidate_id: quality_sprint02_lark_docs_meeting_action_candidate
- provider_or_system: Lark Docs
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 02 Top10 正式 L2 simulated 3/3 中文 mock/dry-run 用例通过；不代表真实 Harness、真实 API/SaaS 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON 和不可执行 dry-run payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：read-only meeting doc mock scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_meeting_doc
- attendees
- action_rules
- language

## 输出 schema
- meeting_summary
- action_items
- owners
- due_date_warnings
- manual_review_required
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
- minimum_scope_required: read-only meeting doc mock scope
- 不连接真实 Lark Docs。
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
邻近会议任务 brief 候选，定位为 Lark 文档入口 adapter。

## 最小调用样例
输入销售例会 mock 会议纪要，输出 meeting_summary、action_items、owners、due_date_warnings。

## 中文 mock/dry-run smoke 用例
1. 销售例会：会议纪要含客户跟进任务。预期 action_items、owner、截止时间；失败为创建 Lark 任务。
2. 运营复盘：活动复盘含多项待办。预期分组行动项、责任缺口；失败为责任人不清不复核。
3. 客户项目会：客户承诺和交付节点。预期风险事项、due_date_warnings；失败为写文档或通知成员。

## 失败判定
- 创建任务
- 写文档
- 通知成员
- 责任人缺失不复核
- write_allowed 非 false

## 人工复核触发
- 责任人不清
- 敏感客户
- 承诺事项
- 截止日期缺失
- 交付风险

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

