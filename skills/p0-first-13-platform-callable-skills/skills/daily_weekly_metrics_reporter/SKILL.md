# daily_weekly_metrics_reporter

## 当前状态

- 状态：可交付平台
- 业务包：经营报表分析包
- 来源项目：Instructor
- 仓库：https://github.com/567-labs/instructor
- 许可证：MIT
- 风险等级：低

## 用途

根据经营指标数据生成日报、周报或月报，输出经营摘要、关键指标、异常说明、可能影响因素和行动建议。

## 输入

- `report_type`：daily、weekly 或 monthly，必填。
- `date_range`：报告周期，必填。
- `metrics`：指标数据列表，必填。
- `metric_definitions`：指标口径，可选但推荐。
- `business_context`：行业、门店、渠道、活动等上下文，可选。
- `comparison_baseline`：同比、环比或目标值，可选。

## 输出

- `executive_summary`：总体摘要。
- `key_metrics`：关键指标。
- `anomalies`：异常列表。
- `likely_factors`：可能影响因素。
- `recommended_actions`：行动建议。
- `data_quality_notes`：数据质量提示。

## 权限与边界

- 只读经营指标。
- 不写入业务系统。
- 不替代审计、税务、授信或重大经营决策。
- 指标口径不清时必须提示补充，不得自行推断。

## 人工复核触发

指标口径不清、数据严重缺失、重大异常、高风险经营决策、财务或税务相关建议。

## 测试与下一步

- L2 状态：simulated L2 passed。
- 平台验收：可交付平台。
- 下一步：平台接入前确认 metrics 输入结构、异常阈值和 data_quality_notes 展示方式。
