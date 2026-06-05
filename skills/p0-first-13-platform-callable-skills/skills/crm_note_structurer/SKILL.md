# crm_note_structurer

## 当前状态

- 状态：可交付平台
- 业务包：销售跟进助手包
- 来源项目：Instructor
- 仓库：https://github.com/567-labs/instructor
- 许可证：MIT
- 风险等级：中

## 用途

将销售跟进记录整理为结构化摘要、客户需求、异议、线索阶段、评分、缺失信息和下一步动作建议。

## 输入

- `raw_note`：销售跟进原始记录，必填。
- `customer_profile`：客户资料，可选。
- `deal_context`：商机上下文，可选。
- `lead_scoring_rules`：线索分层规则，可选。

## 输出

- `summary`：跟进摘要。
- `customer_needs`：客户需求。
- `objections`：客户异议。
- `lead_stage`：线索或商机阶段。
- `lead_score`：线索评分或等级。
- `next_actions`：下一步动作建议。
- `missing_info`：缺失信息。
- `human_review_required`：是否人工复核。

## 权限与边界

- 只读销售记录和可选客户资料。
- 默认不写回 CRM。
- 不虚构客户事实，不自动承诺折扣、合同条款或成交结果。

## 人工复核触发

折扣、合同、投诉、个人信息、付款承诺、客户明确拒绝、低置信度。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前确认销售阶段枚举和是否允许人工确认后写回 CRM。
