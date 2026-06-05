# P0 组件池组合包封装方案

日期：2026-06-03

本文件记录 P0 首批 13 个正式 Skill 之外的组件池组合方案。组件池对象不升级为独立 L3 Skill，只作为后续组合包的增强组件。

## 文档目的与边界

- 目标：说明 3 个组件如何挂靠到已有正式 Skill，形成更完整的业务包能力。
- 不做平台开发，不写 Harness/UI/模型 key 接入代码。
- 不把组件池直接封装成独立 Skill。
- 默认无写权限、无自动发布、无自动外部访问。
- 涉及真实网页访问、抓取、外部服务、CRM/邮箱写入时，必须另走审批和权限边界确认。

## 组件池状态说明

| 组件 ID | 来源项目 | 当前定位 | 独立 L3 状态 |
| --- | --- | --- | --- |
| `seo_keyword_extractor` | KeyBERT | 营销内容包关键词候选组件 | 不独立封装 |
| `data_quality_rules_check` | Great Expectations | 经营报表数据质检前置组件 | 不独立封装 |
| `partner_page_snapshot` | Crawlee | mock/审批后公开页面快照组件 | 不做真实抓取封装 |
| `receipt_line_ocr_extractor` | PaddleOCR | 票据 OCR 结果后处理组件 | 不做完整 OCR 封装 |
| `sales_speaker_timeline` | WhisperX | 会议说话人时间线整理组件 | 不做完整 diarization 封装 |

## 组合包总览

| 组合包 | 包含正式 Skill | 包含组件 | 调用顺序 | 组合输入 | 组合输出 | 权限变化 | 风险与人工复核 | 是否建议落盘为组合方案 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 营销内容生产增强包 | `structured_campaign_brief`, `marketing_copy_pack`, `marketing_compliance_guard` | `seo_keyword_extractor` | 活动 brief 结构化 -> 关键词候选提取 -> 文案包生成 -> 合规检查 | 活动需求、产品信息、目标人群、渠道、语气、禁用词、行业信息 | 结构化 brief、关键词候选、标题/正文/CTA、合规风险、人工复核标记 | 默认无网络、无写权限、不自动发布 | 医疗、金融、教育、夸大承诺、品牌词误用、关键词堆砌触发复核 | 是，优先 |
| 经营报表质检增强包 | `structured_metric_summary`, `daily_weekly_metrics_reporter`, `metric_exception_classifier` | `data_quality_rules_check` | 数据质检 -> 单项指标摘要 -> 异常分类 -> 日报/周报生成 | 指标数据、指标口径、校验规则、异常阈值、报告周期、业务上下文 | 数据质量报告、阻塞问题、指标摘要、异常分类、经营报告、行动建议 | 读取经营指标；无写权限、不自动决策 | 口径不清、缺失数据、金额异常、财务/税务/授信相关建议触发复核 | 是，优先 |
| 票据费用抽取增强包 | `expense_invoice_parser` | `receipt_line_ocr_extractor` | 上游 OCR 文本/行 -> 票据行项目后处理 -> 发票/票据字段抽取 -> 金额一致性与复核提示 | mock OCR 文本、上游 OCR lines、票据字段模板 | 行项目、折扣、税费、金额校验、PII 标记、票据字段、`not_tax_advice` | 只读上游文本；不读真实票据图片、不跑 OCR、不上传 | 低置信 OCR、金额不一致、手机号/会员信息、税务/报销判断触发复核 | 是，但仅作为后处理预案 |
| 销售公开资料简报观察包 | `crm_note_structurer`，可选接 `support_pii_redactor` | `partner_page_snapshot` | mock/审批页面快照 -> PII 检查 -> 销售记录结构化/简报整理 | mock 页面内容或审批 URL 快照、销售记录、客户上下文、来源元数据 | 页面摘要、来源状态、销售跟进摘要、客户关注点、下一步建议 | mock 模式无网络；真实 URL 需单独审批 URL、robots/ToS、访问权限 | 未审批抓取、ToS/robots、版权、隐私、过期页面、客户信息混入触发复核 | 仅作为受限预案 |
| 销售会议纪要增强包 | `crm_note_structurer` | `sales_speaker_timeline` | 上游 diarized segments -> 说话人时间线 -> 关键时刻/行动项归属 -> CRM 记录结构化 | mock diarized segments、会议上下文、销售阶段 | 说话人时间线、行动项归属、风险片段、CRM 摘要、下一步动作 | 只读上游 segments；不读真实音频、不跑 diarization、不上传 | 低置信片段、重叠说话、价格/合同/退款承诺、行动项归属不确定触发复核 | 是，但仅作为上游识别结果整理预案 |

## 营销内容生产增强包

推荐组合：

1. `structured_campaign_brief` 将活动需求整理成结构化 brief。
2. `seo_keyword_extractor` 从产品、受众、卖点中抽取关键词候选。
3. `marketing_copy_pack` 生成标题、正文和 CTA 草稿。
4. `marketing_compliance_guard` 检查敏感行业、绝对化承诺和夸大表达。

输出要求：

- 关键词只作为候选，不承诺 SEO 排名。
- 文案只作为草稿，不自动发布。
- 合规风险必须独立字段输出。
- 医疗、金融、教育、价格敏感和绝对化承诺触发人工复核。

## 经营报表质检增强包

推荐组合：

1. `data_quality_rules_check` 先检查缺失值、范围、重复、口径冲突。
2. `structured_metric_summary` 对单项指标做保守摘要。
3. `metric_exception_classifier` 对异常做分级和核查建议。
4. `daily_weekly_metrics_reporter` 生成日报/周报/月报。

输出要求：

- 数据质量未通过时，报告必须标注阻塞问题。
- 不得把可能原因写成确定归因。
- 不输出税务、审计、授信或重大经营决策结论。
- 关键财务字段缺失、金额异常、口径冲突触发人工复核。

## 票据费用抽取增强包

推荐组合：

1. `receipt_line_ocr_extractor` 接收上游 OCR 文本或 OCR lines，整理行项目、折扣、税费、支付方式、低置信片段和 PII。
2. `expense_invoice_parser` 接收结构化票据文本，抽取发票/票据字段并输出 `not_tax_advice=true`。

当前限制：

- 不读取真实票据图片。
- 不运行 OCR 模型。
- 不下载 PaddleOCR 模型。
- 不上传票据。
- 不输出税务、报销合规或审计结论。

## 销售会议纪要增强包

推荐组合：

1. `sales_speaker_timeline` 接收上游 diarized segments，整理说话人时间线、关键时刻、行动项归属和不确定片段。
2. `crm_note_structurer` 将会议要点和行动项转为 CRM 草稿。

当前限制：

- 不读取真实音频。
- 不运行 diarization 或音频对齐。
- 不下载 pyannote / WhisperX 模型。
- 不使用 Hugging Face token。
- 不把低置信片段写成价格、合同、退款或交付承诺。

## 销售公开资料简报观察包

推荐组合：

1. `partner_page_snapshot` 只接收 mock 页面内容或审批后的公开页面快照。
2. `support_pii_redactor` 可选用于过滤个人联系人、手机号、邮箱等隐私信息。
3. `crm_note_structurer` 将公开页面摘要和销售记录整理成跟进材料。

当前限制：

- 不做真实自由抓取。
- 不登录、不绕验证码、不代理、不批量监控。
- 真实 URL 需指挥中心单独确认 URL 白名单、robots/ToS、访问频率和权限边界。

## 统一权限边界

- `write_actions: none`
- 不自动发布内容。
- 不自动发送邮件或写入 CRM。
- 不默认访问外部网页。
- 不上传客户文档、票据、简历、合同或会议录音到外部服务。

## 统一人工复核触发条件

- 医疗、金融、教育、财税、法律、合同相关建议。
- 退款、赔偿、投诉、账号安全、隐私信息。
- 数据缺失、口径冲突、金额异常、低置信度。
- 未审批网页访问、页面内容可能过期、来源不明。
- 输出将用于对外发布、客户触达、财务报销或合同处理。

## 平台调用验收备注

- 组合包验收应先验证单个 Skill 的 wrapper smoke test，再验证组合调用顺序。
- 组件输出必须作为中间字段记录，便于追踪。
- 任何组件失败时，组合包应返回部分结果和失败原因，不得伪造完整结果。
- 组件池对象不计入当前 13 个正式 Skill 数量。
- `receipt_line_ocr_extractor` 只代表上游 OCR 后处理通过，不代表真实 OCR 通过。
- `sales_speaker_timeline` 只代表上游 diarized segments 整理通过，不代表真实 diarization 通过。

## 待指挥中心决策项

- 是否优先把营销内容生产增强包送平台做组合调用预案。
- 是否优先把经营报表质检增强包送平台做组合调用预案。
- 是否继续将销售公开资料简报观察包限制为 mock/审批后 URL 预案。
