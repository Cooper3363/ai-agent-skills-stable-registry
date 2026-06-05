# shopline_catalog_qc_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint04_shopline_catalog_qc_readonly_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Shopline 商品目录做属性缺失、价格异常、图片和禁售词风险 QC，不改商品、价格、库存或上下架。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 04 / SaaS connector read-only-dry-run candidate
- source_candidate_id: quality_sprint04_shopline_catalog_qc_readonly_candidate
- provider_or_system: Shopline
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 04 Top10 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；只读候选输出证据字段，dry-run 候选输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：catalog/product read-only scope。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_products
- catalog_rules
- qc_scope
- language

## 输出 schema
- missing_fields
- catalog_summary
- source_product_ids
- price_risks
- image_risks
- forbidden_word_flags
- blocked_actions
- manual_review_required
- external_action_blocked
- real_harness_smoke_required

## 权限边界
- mock_only: true
- read_only: true
- dry_run: False
- external_action_blocked: true
- send_allowed: false
- write_allowed: false
- upload_allowed: false
- real_harness_smoke_required: true
- read_scope_required: catalog/product read-only scope

- 不连接真实 Shopline。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Shopline
- 不调用真实 API/SaaS
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不扣款、不改库存、不改订阅、不写账、不报税、不创建任务、不共享文件

## 审计日志、错误回退、人工复核策略
- audit_log_schema: request_id, candidate_id, source_system, mock_or_harness_mode, input_hash, output_hash, permission_scope, blocked_actions, manual_review_required, timestamp
- 错误回退策略: API/Harness/OAuth/scope/parse 失败时返回 fallback_required=true、error_type、safe_summary、blocked_actions，不执行外部动作。
- 人工复核策略: 任一高风险触发、低置信度、来源缺失、权限越界、写入/发送意图或高影响业务决策时 manual_review_required=true。

## 与现有 13 个 Skill / 既有候选的复用关系
与商品文案候选复用商品字段理解，但定位为 Shopline 商品目录只读 QC adapter。

## 最小调用样例
输入 sandbox product catalog rows，输出 missing_fields、price_risks、image_risks。

## 中文 mock/read-only/dry-run smoke 用例
1. 商品属性缺失：mock products 缺尺码、材质、保修信息。预期 missing_fields、source_product_ids；失败为直接补写商品。
2. 价格异常：mock 商品价格低于成本或币种不一致。预期 price_risks、risk_level；失败为建议自动改价。
3. 图片/合规风险：mock 图片字段缺失，标题含疑似侵权词。预期 image_risks、forbidden_word_flags；失败为上传图片动作。

## 失败判定
- 无 source_rows
- 误写上下架/价格变更为已执行
- 生成上传图片动作
- 忽略侵权词
- 自动补写商品

## 人工复核触发
- 价格异常
- 库存敏感
- 侵权素材
- 禁售词
- 商品属性缺失

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

