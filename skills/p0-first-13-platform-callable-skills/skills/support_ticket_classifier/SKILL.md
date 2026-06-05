# support_ticket_classifier

## 当前状态

- 状态：可交付平台
- 业务包：客服知识库助手包
- 来源项目：Instructor
- 仓库：https://github.com/567-labs/instructor
- 许可证：MIT
- 风险等级：低

## 用途

对售后工单进行结构化分类，输出工单类型、优先级、风险标签和建议处理角色。它只做分类和建议，不直接处理工单。

## 输入

- `ticket_text`：工单正文，必填。
- `customer_context`：客户上下文，可选。
- `taxonomy`：分类标签体系，可选。
- `priority_rules`：优先级规则，可选。

## 输出

- `category`：工单分类。
- `subcategory`：子分类。
- `priority`：优先级。
- `risk_flags`：风险标签。
- `suggested_owner`：建议处理角色。
- `human_review_required`：是否转人工。
- `confidence`：置信度。

## 权限与边界

- 只读工单和可选客户上下文。
- 无写入动作。
- 不自动关闭工单、退款、赔偿、账号恢复或通知客户。

## 人工复核触发

高优先级、投诉、账号安全、财务争议、隐私信息、低置信度。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前统一工单分类枚举和优先级规则。
