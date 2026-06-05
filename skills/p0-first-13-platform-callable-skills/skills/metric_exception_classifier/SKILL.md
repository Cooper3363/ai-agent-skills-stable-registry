# metric_exception_classifier

## 当前状态

- 状态：可交付平台
- 业务包：经营报表分析包
- 来源项目：Marvin
- 仓库：https://github.com/PrefectHQ/marvin
- 许可证：Apache-2.0
- 风险等级：中

## 用途

对经营指标异常进行分级分类，给出可能原因和核查建议。它只做辅助判断，不输出确定归因，也不自动执行经营动作。

## 输入

- `metric_name`：指标名，必填。
- `current_value`：当前值，必填。
- `baseline_value`：基线值，必填。
- `threshold_rules`：异常阈值规则，必填。
- `business_context`：业务上下文，可选。
- `recent_events`：近期活动、系统变更、节假日等，可选。

## 输出

- `exception_detected`：是否异常。
- `severity`：严重等级。
- `exception_type`：异常类型。
- `possible_causes`：可能原因。
- `verification_steps`：核查步骤。
- `human_review_required`：是否人工复核。
- `confidence`：置信度。

## 权限与边界

- 只读经营指标。
- 不写入业务系统。
- 不做确定归因，不做自动处置。

## 人工复核触发

重大现金流或收入异常、疑似系统故障、低置信度高影响、指标口径变化、财务/税务相关场景。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前确认阈值规则解释和人工复核路由。
