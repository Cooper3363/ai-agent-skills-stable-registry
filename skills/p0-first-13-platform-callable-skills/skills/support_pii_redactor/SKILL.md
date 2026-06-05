# support_pii_redactor

## 当前状态

- 状态：可交付平台
- 业务包：客服知识库助手包
- 来源项目：Microsoft Presidio
- 仓库：https://github.com/microsoft/presidio
- 许可证：MIT
- 风险等级：低
- L2 状态：simulated L2 passed
- 平台调用验收：passed

## 用途

对中文客服文本中的敏感个人信息进行脱敏，返回脱敏文本、实体列表、风险等级和人工复核建议。

## 输入

- `text`：待脱敏中文客服文本，必填。
- `entity_rules`：实体识别规则配置，可选。
- `masking_policy`：脱敏策略，可选，例如订单号保留后四位。
- `language`：默认 `zh-CN`。

## 输出

- `redacted_text`：脱敏后的文本。
- `entities`：命中的实体列表，包含 `type`、`replacement`、`confidence`、`rule_id`。
- `risk_level`：风险等级。
- `manual_review_required`：是否人工复核。
- `notes`：说明与风险提示。

## 权限与边界

- 只读客服文本。
- 不上传对话。
- 不写入客户系统。
- 不做身份判断、账号归属判断或账号恢复。
- 运行方式为本地规则增强或平台受控运行，不需要外部 API key。

## 支持实体

`PHONE_CN`、`EMAIL`、`CN_ID`、`ADDRESS_CN`、`ORDER_ID`、`WECHAT_ID`、`NAME`。

## 人工复核触发

高敏 PII、账号恢复、投诉工单、实体识别低置信度、脱敏冲突、疑似漏脱敏。

## 接入备注

平台同事接入前需按 `SMOKE_TEST_PLAN.md` 做真实 wrapper smoke test，重点验证中文地址、身份证 `X/x`、订单号后四位保留、禁止外部上传和低置信度复核。
