# faq_answer_with_citations

## 当前状态

- 状态：可交付平台
- 业务包：客服知识库助手包
- 来源项目：Haystack
- 仓库：https://github.com/deepset-ai/haystack
- 许可证：Apache-2.0
- 风险等级：中

## 用途

根据企业知识库回答客户问题，并返回可追溯引用、置信度和转人工判断。引用只能来自输入知识库中实际命中的条目。

## 输入

- `question`：客户问题，必填。
- `knowledge_base_refs`：可检索知识库或文档集合，必填。
- `customer_context`：客户状态、订单或会员信息，可选。
- `language`：默认 zh-CN。
- `min_confidence`：最低置信度，默认 0.65。

## 输出

- `answer`：面向客户的中文回答。
- `citations`：引用来源列表。
- `confidence`：置信度。
- `handoff_required`：是否转人工。
- `handoff_reason`：转人工原因。
- `risk_flags`：风险标签。

## 权限与边界

- 必须读取知识库。
- 可选读取客户上下文。
- 无写入动作。
- 知识库缺失或低置信度时不得编造答案。

## 人工复核触发

低置信度、无引用、引用冲突、投诉、退款、账号恢复、隐私敏感、知识库缺失。

## 测试与下一步

- L2 状态：专项 simulated Harness 复测通过。
- 平台验收：可交付平台。
- 下一步：平台接入前确认 citation 字段能回溯到真实知识库命中。
