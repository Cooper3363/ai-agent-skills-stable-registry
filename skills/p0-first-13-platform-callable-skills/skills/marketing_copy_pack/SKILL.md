# marketing_copy_pack

## 当前状态

- 状态：可交付平台
- 业务包：营销内容生产包
- 来源项目：Marketing Skills
- 仓库：https://github.com/coreyhaines31/marketingskills
- 许可证：MIT
- 风险等级：低

## 用途

为中小微企业生成可编辑的中文营销文案包，包括标题、正文、CTA、修改建议和合规风险提示。默认只生成草稿，不自动发布到任何平台。

## 输入

- `business_name`：企业或品牌名，必填。
- `product_or_service`：产品或服务说明，必填。
- `target_audience`：目标客户，必填。
- `channel`：发布渠道，必填。
- `campaign_goal`：活动目标，必填。
- `tone`：语气风格，可选。
- `constraints`：禁用词、必须包含信息、字数限制，可选。

## 输出

- `headlines`：标题候选。
- `body_copy`：正文候选。
- `ctas`：行动按钮或结尾话术。
- `compliance_notes`：合规风险提示。
- `revision_suggestions`：修改建议。

## 权限与边界

- 不需要网络权限。
- 不需要写入或发布权限。
- 不承诺投放效果，不替代广告平台审核或法务审核。

## 人工复核触发

医疗、金融、教育、未成年人、绝对化承诺、保证收益、价格敏感、素材来源不明。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前做 smoke test，确认输出字段、渠道字数限制和合规提示能被前端展示。
