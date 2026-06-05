# support_reply_guardrail

## 当前状态

- 状态：可交付平台
- 业务包：客服知识库助手包
- 来源项目：Guardrails
- 仓库：https://github.com/guardrails-ai/guardrails
- 许可证：Apache-2.0
- 风险等级：中

## 用途

检查客服拟发送回复是否存在退款承诺、赔偿承诺、账号安全越界、投诉升级、隐私泄露或低置信度风险。

## 输入

- `reply_text`：客服拟发送回复，必填。
- `customer_message`：客户原始问题，必填。
- `policy_context`：企业客服政策摘要，可选。
- `ticket_context`：工单上下文，可选。
- `risk_threshold`：风险阈值，可选。

## 输出

- `passed`：是否通过检查。
- `risk_level`：风险等级。
- `violations`：违规项。
- `suggested_revision`：安全改写建议。
- `human_review_required`：是否人工复核。
- `review_reason`：复核原因。

## 权限与边界

- 只读客户消息、客服回复和可选政策上下文。
- 不替客服承诺退款、赔偿、账号恢复或处理投诉。
- 只检查不处置。

## 人工复核触发

退款承诺、投诉升级、账号安全、隐私信息、低置信度、政策缺失、客户情绪激烈。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前验证误报/漏报和建议改写是否足够保守。
