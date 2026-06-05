# support_kb_doc_cleaner

## 当前状态

- 状态：可交付平台
- 业务包：客服知识库助手包
- 来源项目：Unstructured
- 仓库：https://github.com/Unstructured-IO/unstructured
- 许可证：Apache-2.0
- 风险等级：中
- L2 状态：simulated L2 passed
- 平台调用验收：passed

## 用途

在客服知识库构建前，对本地或 mock 文本文档进行清洗、分块、章节识别和质量检查，输出可进入检索知识库的 `cleaned_chunks`。

## 输入

- `document_text`：待清洗文档文本，必填。
- `source_type`：如 `faq_html`、`pdf_text`、`policy_text`、`mixed_text`，必填。
- `source_metadata`：来源元数据，可选。
- `cleaning_rules`：清洗规则，可选。
- `chunking_policy`：分块策略，可选。

## 输出

- `cleaned_chunks`：清洗后的分块列表。
- `removed_noise`：移除的噪声说明。
- `detected_sections`：检测到的章节结构。
- `quality_warnings`：质量提示。
- `manual_review_required`：是否人工复核。

## 权限与边界

- 只处理本地或 mock 文本。
- 不接 Unstructured Platform/API。
- 不启用云 OCR。
- 不上传客户文档。
- 本轮不声明真实 PDF/OCR 通用解析能力。
- 运行方式为本地文本处理或平台受控运行，不需要外部 API key。

## 人工复核触发

来源不明文档、合同/隐私内容、章节混杂、引用来源缺失、清洗质量低、疑似 OCR 错误严重。

## 接入备注

平台同事接入前需按 `SMOKE_TEST_PLAN.md` 做真实 wrapper smoke test。该 Skill 可与 `faq_answer_with_citations` 衔接，但平台侧只能传入已本地化文本内容。
