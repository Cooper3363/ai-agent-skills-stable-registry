# structured_campaign_brief

## 当前状态

- 状态：可交付平台
- 业务包：营销内容生产包
- 来源项目：Instructor
- 仓库：https://github.com/567-labs/instructor
- 许可证：MIT
- 风险等级：低

## 用途

把非结构化活动需求整理为营销活动 brief、素材需求、投放变量和缺失字段，供后续内容生产或人工投放配置使用。

## 输入

- `raw_brief`：非结构化活动需求，必填。
- `business_context`：企业、产品、行业、门店等上下文，可选。
- `channel`：投放或发布渠道，可选。
- `campaign_goal`：活动目标，可选。
- `constraints`：预算、时间、库存、禁用词等约束，可选。

## 输出

- `campaign_summary`：活动摘要。
- `target_audience`：目标人群。
- `key_messages`：核心卖点。
- `creative_requirements`：素材需求。
- `placement_variables`：投放变量。
- `missing_fields`：缺失字段。
- `human_review_required`：是否人工复核。

## 权限与边界

- 不需要网络权限。
- 不写入广告平台。
- 不自动发布、不自动改预算、不承诺投放效果。

## 人工复核触发

价格不清、库存不清、敏感行业、预算异常、活动规则冲突。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前确认渠道变量结构和缺失字段提示方式。
