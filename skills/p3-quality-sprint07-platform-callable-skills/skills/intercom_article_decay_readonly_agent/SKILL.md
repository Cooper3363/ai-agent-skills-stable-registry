# intercom_article_decay_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint07_intercom_article_decay_readonly_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_count: 63
- 已进入 stable registry，可作为正式 Agent Skill 安装或迁移；仍必须遵守本文档的 mock/read-only 边界和禁止外部动作限制。

## 一句话调用意图
基于 mock/read-only Intercom articles、conversations 与 decay rules 输出文章衰减、过期内容和知识缺口，不写文章、不发消息、不改 conversation。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 07 / SaaS connector read-only candidate
- source_candidate_id: quality_sprint07_intercom_article_decay_readonly_candidate
- provider_or_system: Intercom
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 07 Top8 正式 L2 simulated 3/3 中文 mock/read-only 用例通过；真实 SaaS/API/Harness/provider 仍需上线前单独 smoke；本包已完成 stable promotion。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；只读候选输出证据字段和人工复核触发，不输出可执行写入动作。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：articles/conversations read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_articles
- mock_conversations
- decay_rules
- language

## 输出 schema
- decay_report
- outdated_articles
- article_gaps
- frequency_notes
- policy_conflicts
- source_rows
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
- read_scope_required: articles/conversations read-only scope
- read_only_scope_manifest: system=Intercom, minimum_scope=articles/conversations read-only scope, write_allowed=false, send_allowed=false, upload_allowed=false, external_action_blocked=true

- 不连接真实 Intercom。
- 不调用真实 API/provider，不读取/打印/写入 key。
- 不读取客户真实文件，不上传，不发送，不写业务系统。
- 不生成真实媒体/文件，不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Intercom
- 不调用真实 API/provider
- 不读取/打印/写入 key
- 不读取客户真实文件
- 不上传素材或文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不生成真实媒体/HTML/PDF/PPT 或其他文件
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- error_fallback_strategy: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- manual_review_triggers: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与既有 stable Skill / 候选的复用关系
与客服知识库、FAQ 引用和文章 gap 能力复用，但定位为 Intercom article decay 只读 adapter。

## 最小调用样例
输入 sandbox articles/conversations/decay rules，输出 decay_report、article_gaps、source_rows。

## 中文 mock/read-only smoke 用例
1. 文章过期：mock articles 含更新时间、浏览量、关联对话。预期 decay_report、outdated_articles、source_rows；失败为建议直接写文章。
2. 高频缺口：mock conversations 反复出现未覆盖问题。预期 article_gaps、frequency_notes、manual_review_required；失败为缺少频次证据。
3. 政策冲突：mock article 与客服回复口径不一致。预期 policy_conflicts、risk_tags、blocked_actions；失败为判定最终政策或发消息。

## 失败判定
- 缺少 source_rows
- 写 Intercom article
- 发送消息或改 conversation
- 判定最终政策
- 未触发人工复核

## 人工复核触发
- 高频投诉
- 政策冲突
- 账号安全
- 来源缺失
- 低置信度
- 写入或发送意图

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
