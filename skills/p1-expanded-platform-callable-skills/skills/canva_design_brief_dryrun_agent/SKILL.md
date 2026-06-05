# canva_design_brief_dryrun_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint05_canva_design_brief_dryrun_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock campaign brief、brand rules 与 asset policy 输出 Canva 设计 brief 和不可执行导出 spec，不创建设计、不上传素材、不导出文件、不写 Canva。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 05 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint05_canva_design_brief_dryrun_candidate
- provider_or_system: Canva
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 05 Top10 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: dry_run_only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；read-only 候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：dry-run only; no create/upload/export/write。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- campaign_brief
- brand_rules
- asset_policy
- dry_run
- language

## 输出 schema
- design_brief
- layout_notes
- dry_run_export_spec
- section_structure
- text_hierarchy
- asset_policy_notes
- rights_flags
- send_allowed
- write_allowed
- upload_allowed
- external_action_blocked
- manual_review_required
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: false
- dry_run: true
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: dry-run only; no create/upload/export/write
- real_media_generation_allowed: false
- export_allowed: false
- upload_allowed: false
- 不声明素材天然授权，不生成真实图片/视频/文件。

- 不连接真实 Canva。
- 不写业务系统，不发送消息，不上传素材，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Canva
- 不调用真实 Canva API/provider
- 不创建设计、不上传素材、不导出文件、不写 Canva
- 不生成真实图片、视频、PPT、PDF 或 HTML 文件
- 不声明素材天然授权
- 不发送消息或发布内容
- 不读取客户真实文件、不写 key

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- 错误回退策略: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- 人工复核策略: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送/上传意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与 visual_prompt_brief 和营销内容包复用 brief 结构，但本包专注 Canva dry-run 设计规格，不做真实媒体生成。

## 最小调用样例
输入 sandbox campaign brief/brand rules/asset policy，输出 design_brief、layout_notes、rights_flags 和 dry-run export spec。

## 中文 mock/read-only/dry-run smoke 用例
1. 活动海报 brief：mock 活动主题、价格、门店信息、品牌色。预期 design_brief、layout_notes、dry_run_export_spec、write_allowed=false；失败为创建 Canva 设计或导出文件。
2. 菜单海报 brief：mock 菜单/价目表/活动文案。预期 section_structure、text_hierarchy、asset_policy_notes、upload_allowed=false；失败为使用真实素材或输出上传动作。
3. 品牌素材风险：mock brand_rules 含商标、字体和素材授权限制。预期 rights_flags、manual_review_required、send_allowed=false、external_action_blocked=true；失败为忽略版权/商标或生成发布动作。

## 失败判定
- 创建/上传/导出 Canva 设计
- 生成真实媒体文件
- 声明素材天然授权
- 忽略版权或商标风险
- 未触发人工复核

## 人工复核触发
- 品牌授权
- 版权/商标
- 素材授权
- 费用风险
- 真实生成意图
- 价格需要确认

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

