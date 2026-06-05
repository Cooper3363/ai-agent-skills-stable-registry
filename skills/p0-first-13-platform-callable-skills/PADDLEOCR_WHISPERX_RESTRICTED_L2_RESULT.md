# PaddleOCR 与 WhisperX 受限 L2 simulated 测试结果

日期：2026-06-03

本文件记录 P0 补充候选 PaddleOCR 与 WhisperX 的严格受限 L2 simulated 测试结果。两者本轮只验证真实识别结果之后的结构化处理能力，不验证真实 OCR、真实音频转写、真实 diarization 或模型下载运行能力。

## 执行边界

- 未安装依赖。
- 未下载模型。
- 未访问外网。
- 未读取真实图片、票据或音频。
- 未运行真实 OCR。
- 未运行真实 diarization 或音频对齐。
- 未请求权限。

## 总体结论

| 候选 | Skill ID | L2 结论 | 当前定位 | 下一步 |
| --- | --- | --- | --- | --- |
| PaddleOCR | `receipt_line_ocr_extractor` | L2 通过但仅作为组件 | 票据 OCR 结果后处理组件 | 进入票据/费用包组件池；完整 OCR 需真实本地票据图片 Harness |
| WhisperX | `sales_speaker_timeline` | L2 通过但仅作为组件 | 会议说话人时间线整理组件 | 进入销售会议包组件池；完整 diarization 需真实本地短音频 Harness |

## 结果总表

| 候选 | 3 个中文用例是否完成 | 输出结构稳定性 | 中文可用性 | 权限边界 | 人工复核触发 | 建议下一步 |
| --- | --- | --- | --- | --- | --- | --- |
| PaddleOCR / `receipt_line_ocr_extractor` | 完成 | 中高 | 中高 | 只用 mock OCR 文本/receipt lines；不读真实图片，不跑 OCR，不下载模型 | 低置信 OCR、金额不一致、票据含 PII、报销/税务判断 | 作为票据 OCR 后处理组件；不进入独立 L3 |
| WhisperX / `sales_speaker_timeline` | 完成 | 高 | 高 | 只用 mock diarized segments；不读真实音频，不跑 diarization/对齐，不下载模型 | 说话人低置信、重叠说话、价格/合同承诺、行动项归属不确定 | 作为会议说话人时间线整理组件；不进入独立 L3 |

## PaddleOCR / `receipt_line_ocr_extractor`

推荐固定输出字段：

- `merchant`
- `receipt_date`
- `line_items`
- `total_amount`
- `discounts`
- `tax_amount`
- `payment_method`
- `confidence`
- `ocr_quality_notes`
- `validation_flags`
- `pii_entities`
- `manual_review_required`
- `not_tax_advice`

### 用例 A：餐饮小票完整行项目

输入：mock OCR 文本，包含商户、日期、3 个行项目、合计、微信支付。

预期：

- 能抽取 `merchant=幸福餐厅`、日期、三行 `line_items`、`total_amount=60.00`。
- `validation_flags=[]`。
- `manual_review_required=false`。

失败点：

- 数量/单价/金额列错位。
- 把支付方式当商品。
- 漏日期。

### 用例 B：OCR 低置信与数字疑似误识别

输入：mock receipt lines，包含低置信 `3.O0`、`1O.00`。

预期：

- `ocr_quality_notes` 标注 O/0、1/I/0 疑似混淆。
- `manual_review_required=true`。
- `validation_flags` 包含 `low_ocr_confidence`。

失败点：

- 把低置信数字直接当事实。
- 不提示人工复核。
- 金额校验缺失。

### 用例 C：折扣、税费、手机号隐私

输入：mock OCR 文本，包含商品、满减、税费、实付和会员手机号。

预期：

- 折扣进入 `discounts`，不进入商品行。
- `tax_amount=1.50`，`total_amount=26.50`。
- `pii_entities=[PHONE_CN]`。
- `not_tax_advice=true`。
- 票据含 PII 时触发复核或风险提示。

失败点：

- 把折扣当商品。
- 总额计算不一致未提示。
- 手机号原文泄露。

### 当前定位

PaddleOCR 不进入独立 L3。当前只作为票据 OCR 后处理组件，承接 `mock_ocr_text` 或上游 OCR lines。若未来要成为完整 OCR Skill，必须另做真实本地票据图片 Harness。

## WhisperX / `sales_speaker_timeline`

推荐固定输出字段：

- `speaker_timeline`
- `speaker_labels`
- `key_moments`
- `action_items_by_speaker`
- `uncertainty_segments`
- `diarization_quality`
- `risk_flags`
- `manual_review_required`

### 用例 A：销售/客户双人清晰对话

输入：mock diarized segments，销售与客户对话，客户确认排课提醒和签到统计需求，销售约下周二演示。

预期：

- `speaker_timeline` 按时间排序。
- `key_moments` 包含需求确认。
- `action_items_by_speaker` 中销售负责“下周二演示”。
- `diarization_quality=high`。
- `manual_review_required=false`。

失败点：

- 行动项归属到客户。
- 时间顺序错乱。
- 漏掉客户需求。

### 用例 B：多人会议与说话人切换

输入：mock diarized segments，销售A、客户老板、客户财务三方对话，包含价格异议和付款周期。

预期：

- 保留三类角色标签。
- `key_moments` 包含价格异议、付款周期。
- `risk_flags` 包含 `contract_terms` / `payment_terms`。
- `manual_review_required=true`。

失败点：

- 混淆客户老板和客户财务。
- 漏合同付款周期风险。
- 生成超权限承诺。

### 用例 C：重叠说话与低置信片段

输入：mock diarized segments，包含重叠说话和低置信“可以……[听不清]”片段。

预期：

- `uncertainty_segments` 标注重叠/低置信片段。
- `risk_flags=[refund_sensitive, low_confidence_segment]`。
- 只记录“让主管确认”，不得生成退款承诺。
- `manual_review_required=true`。

失败点：

- 把听不清片段补成承诺。
- 不标重叠说话。
- speaker label 不确定时仍强行归属。

### 当前定位

WhisperX 不进入独立 L3。当前只作为会议说话人时间线整理组件，承接上游 diarized segments。若未来要成为完整 diarization Skill，必须另做真实本地短音频 Harness，并单独处理 pyannote/Hugging Face user agreement、token、模型许可证和会议录音隐私边界。

## 指挥中心决策记录

- PaddleOCR / `receipt_line_ocr_extractor`：进入组件池，不进入独立 L3。
- WhisperX / `sales_speaker_timeline`：进入组件池，不进入独立 L3。
- 二者不能被标记为真实 OCR/真实 diarization 通过。
- 后续如要独立封装，必须先通过真实 Harness 补测与权限/模型/数据边界复核。
