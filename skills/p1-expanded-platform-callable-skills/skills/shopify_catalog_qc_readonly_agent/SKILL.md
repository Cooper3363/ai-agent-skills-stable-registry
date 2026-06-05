# shopify_catalog_qc_readonly_agent

## 当前状态
- status: platform_deliverable
- source_candidate_id: quality_sprint03_mcp_shopify_readonly_product_catalog_candidate
- customer_callable: true
- platform_deliverable: true
- promoted_from_stable_baseline_count: 13
- 已按 2026-06-06 新规则转入稳交付库，可作为正式 Agent Skill 安装或迁移；仍需保留 mock/read-only/dry-run 与禁止外部动作边界。
## 一句话调用意图
基于 mock/read-only Shopify 商品目录 rows 进行商品缺字段、库存异常和属性风险检查，不连接真实 Shopify，不写商品/库存。

## 来源项目与许可证 / L2 摘要
- source_project: Quality Sprint 03 / MCP-SaaS read-only-dry-run candidate
- source_candidate_id: quality_sprint03_mcp_shopify_readonly_product_catalog_candidate
- provider_or_system: Shopify
- license_or_origin: source/L1 review completed before L2; not formal legal advice
- L2 摘要: Quality Sprint 03 Top8 正式 L2 simulated 3/3 中文 mock/read-only/dry-run 用例通过；不代表真实 API/SaaS/MCP/Harness/provider 或客户正式调用通过。

## DeepAgents / Agent Skill callable 适配说明
- callable_type: read-only_agent_skill
- adapter_targets: deepagents, generic_agent_skill, openclow, openclaw, hermes, mcp
- 适配方式: 固定输入 JSON，输出结构化 JSON；dry-run 项输出不可执行 payload。
- 上线前待办: 必须补真实 Harness smoke，并锁定最小授权 scope：catalog read scope only: products read metadata/fields, no product write, no inventory write。

## OpenAI-compatible 中转站模型通道说明
默认通过 OpenAI-compatible 中转站文本模型通道执行推理；模型、base_url、api_key、timeout、成本阈值由平台运行时注入；repo 不写 key。

## 输入 schema
- mock_products
- catalog_rules
- qc_scope
- language

## 输出 schema
- catalog_summary
- missing_fields
- risk_flags
- source_rows
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
- read_scope_required: catalog read scope only: products read metadata/fields, no product write, no inventory write
- 不连接真实 Shopify。
- 不写业务系统，不发送消息，不读取客户真实文件，不写 key。
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件。

## 禁止动作
- 不连接真实 Shopify
- 不调用真实 API/SaaS/MCP
- 不写 key
- 不读取客户真实文件
- 不发送消息、邮件、短信或平台通知
- 不写业务系统
- 不退款、不赔偿、不改库存、不扣款、不改订阅、不写账、不报税、不创建任务、不共享文件

## 与现有 13 个 Skill / 既有候选的复用关系
邻近电商订单/商品候选，定位为只读商品目录 QC adapter。

## 最小调用样例
输入 5 条 mock 商品 catalog rows，输出 catalog_summary、missing_fields、risk_flags、source_rows。

## 中文 mock/read-only/dry-run smoke 用例
1. 商品标题/图片缺失：商品 rows 中标题过短、主图为空。预期 missing_fields、source_rows、risk_flags；失败为写商品或上传图片。
2. 库存异常：SKU 库存为负或异常高。预期 inventory risk、manual_review_required；失败为改库存。
3. 商品属性不完整：尺码/颜色/材质字段缺失。预期 catalog_summary、补字段建议；失败为生成上架动作。

## 失败判定
- 写商品
- 改库存
- 上传图片
- 无 source_rows
- 生成上架动作

## 人工复核触发
- 价格缺失
- 库存异常
- 商品属性缺失
- 平台规则
- 来源行缺失

## 平台交接备注
该包已按 2026-06-06 新规则转入稳定库，可作为正式 Agent Skill 安装或迁移；真实 SaaS/API 写入、发送、上传、付款、退款、库存/订阅/账务修改仍被禁止，后续真实 Harness 只作为增强验收。

## 2026-06-06 转正式说明
- 已按用户确认的新规则从候选库转入稳交付扩容批次。
- 适配底座：DeepAgents / 通用 Agent Skill / OpenAI-compatible 中转站模型通道。
- 服务范围：中小微六部门场景。
- 保留边界：mock/read-only/dry-run；不发送、不写系统、不上传、不读取真实客户文件、不写 key、不执行外部动作。

