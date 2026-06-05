# expense_invoice_parser

## 当前状态

- 状态：可交付平台
- 业务包：经营报表/财票分析包
- 来源项目：invoice2data
- 仓库：https://github.com/invoice-x/invoice2data
- 许可证：MIT
- 风险等级：中
- L2 状态：simulated L2 passed
- 平台调用验收：passed

## 用途

从本地或 mock 费用票据/OCR 文本中抽取票据类型、主体、金额、税额、日期和明细。它不提供税务、报销合规或审计结论。

## 输入

- `invoice_text`：发票、收据或 OCR 文本，必填。
- `source_type`：如 `invoice_text`、`receipt_text`、`ocr_text`，必填。
- `locale`：默认 `zh-CN`。
- `extraction_templates`：票据模板规则，可选。

## 输出

- `invoice_type`：票据类型。
- `seller_name`：销售方或商户名称。
- `buyer_name`：购买方名称。
- `invoice_number`：发票号。
- `invoice_date`：开票日期。
- `total_amount`：总金额。
- `tax_amount`：税额。
- `currency`：币种。
- `line_items`：明细项。
- `confidence`：置信度。
- `missing_fields`：缺失字段。
- `manual_review_required`：是否人工复核。
- `not_tax_advice`：固定为 `true`。

## 权限与边界

- 只处理本地或 mock 发票/OCR 文本。
- 不上传票据。
- 不启用 Google Cloud Vision 或云 OCR。
- 不输出税务、报销合规或审计结论。
- 不作为报销审批结论。
- 运行方式为本地文本规则或平台受控运行，不需要外部 API key。

## 人工复核触发

金额不一致、税号/发票号缺失、票据类型不确定、低置信度、涉及报销/税务判断、餐饮小票误判风险。

## 接入备注

平台同事接入前需按 `SMOKE_TEST_PLAN.md` 做真实 wrapper smoke test，重点验证金额一致性、票据类型边界、`not_tax_advice=true` 和人工复核触发。
