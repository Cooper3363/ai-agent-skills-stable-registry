# structured_metric_summary

## 当前状态

- 状态：可交付平台
- 业务包：经营报表分析包
- 来源项目：Instructor
- 仓库：https://github.com/567-labs/instructor
- 许可证：MIT
- 风险等级：中

## 用途

对单项或字段级经营指标生成结构化摘要、变化说明、异常提示和保守行动建议。它不生成完整日报、周报或月报。

## 输入

- `metric_name`：指标名称，必填。
- `current_value`：当前值，必填。
- `comparison_value`：对比值，可选。
- `metric_definition`：指标口径，可选。
- `threshold_rules`：异常阈值，可选。
- `business_context`：业务上下文，可选。

## 输出

- `metric_summary`：指标摘要。
- `change_description`：变化说明。
- `anomaly_flag`：是否异常。
- `anomaly_reasoning`：异常说明。
- `possible_factors`：可能因素。
- `recommended_actions`：行动建议。
- `data_quality_notes`：数据质量提示。
- `human_review_required`：是否人工复核。

## 权限与边界

- 只读经营指标。
- 不替代完整报表生成、审计、税务、授信或财务决策。
- 不把相关性写成确定因果。

## 人工复核触发

财务决策、税务、授信、现金流重大异常、指标口径不清、数据缺失严重。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前确认它与 `daily_weekly_metrics_reporter` 的边界。
