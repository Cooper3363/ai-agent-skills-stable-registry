# marketing_compliance_guard

## 当前状态

- 状态：可交付平台
- 业务包：营销内容生产包
- 来源项目：Guardrails
- 仓库：https://github.com/guardrails-ai/guardrails
- 许可证：Apache-2.0
- 风险等级：中

## 用途

检查营销文案中的禁词、夸大承诺、医疗疗效、金融收益、教育结果承诺、价格误导和敏感人群风险。

## 输入

- `copy_text`：待检查营销文案，必填。
- `industry`：行业，可选。
- `channel`：发布渠道，可选。
- `compliance_rules`：合规规则或禁词表，可选。
- `target_audience`：目标人群，可选。

## 输出

- `passed`：是否通过。
- `risk_level`：风险等级。
- `risk_items`：风险项。
- `suggested_rewrites`：修改建议。
- `human_review_required`：是否人工复核。
- `disclaimer`：合规提示。

## 权限与边界

- 只读营销文案。
- 不自动发布。
- 不替代法务审核、广告平台审核或监管判断。

## 人工复核触发

医疗疗效、金融收益、教育承诺、绝对化用语、敏感人群、误导性价格。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前补行业规则词库和客户自定义禁用表达。
