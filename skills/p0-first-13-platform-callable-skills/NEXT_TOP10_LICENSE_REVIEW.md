# Next Top 10 license and commercial risk review

Date: 2026-06-03

Scope: Product-screening license and commercial risk review for the next Top 10 candidates. This is not formal legal advice.

## Summary

| Result | Count | Candidates |
| --- | ---: | --- |
| Passed, send to test bench | 6 | DeepEval, Microsoft Presidio, Camelot, Whisper, Ragas, Unstructured |
| Needs dependency/model review | 2 | PaddleOCR, WhisperX |
| Deferred | 1 | Crawlee real competitor price monitoring |
| Needs legal/security review | 1 | Composio |

## Review Table

| Candidate | Skill ID | Main license result | Dependency/API/model/data risk | Direct Skill? | Risk | Review result | Next step |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DeepEval | `support_response_eval_suite` | Apache-2.0 | LLM-as-judge depends on model/API; cloud trace or hosted platform must be disabled or separately reviewed | Yes, local eval/user key only | Medium | Passed | Send to test bench |
| Microsoft Presidio | `hr_resume_privacy_masker` | MIT | Resume data contains sensitive PII; optional NLP/OCR models need local-only boundary | Yes | Medium | Passed | Send to test bench |
| Crawlee | `competitor_price_change_monitor` | Apache-2.0 | Real scraping, proxies, bot protection, robots/ToS, price monitoring frequency risk | Not as direct real scraping Skill | High | Deferred | Defer real monitoring; keep mock/approved snapshot only |
| Camelot | `quote_pdf_table_extractor` | MIT | PDF table extraction acceptable if non-OCR/non-ML path; OCR/ML optional deps need separate review | Yes, local PDF/table text only | Medium | Passed | Send to test bench |
| PaddleOCR | `receipt_line_ocr_extractor` | Apache-2.0 | OCR/model weights, training data, download source, Paddle deps, receipt image privacy need lock-down | Not yet | Medium | Needs dependency/model review | Supplement review |
| Whisper | `sales_meeting_transcriber` | MIT | Meeting recording consent and customer/sales data privacy; local inference boundary required | Yes, local audio/model only | Medium | Passed | Send to test bench |
| WhisperX | `sales_speaker_timeline` | BSD-2-Clause | Diarization may need pyannote/Hugging Face token and model user agreement; meeting privacy risk | Not yet | Medium | Needs dependency/model review | Supplement review |
| Ragas | `support_rag_eval_runner` | Apache-2.0 | Uses model APIs in examples; telemetry/usage tracking must be disabled; eval samples need redaction | Yes, redacted samples/user key only | Medium | Passed | Send to test bench |
| Unstructured | `contract_section_partitioner` | Apache-2.0 | Must separate open-source library from Platform/API; local parsing only; contract text sensitive | Yes, local text only, no legal conclusion | Medium | Passed | Send to test bench |
| Composio | `sales_followup_action_connector` | MIT | OAuth, Gmail/CRM/calendar actions, real send/write, scopes, audit, revocation, third-party API terms | No, connector only | High | Needs legal/security review | Legal/security review |

## Test Bench Batch

The following 6 candidates were sent to the test bench for L2 simulated testing:

1. `support_response_eval_suite`
2. `hr_resume_privacy_masker`
3. `quote_pdf_table_extractor`
4. `sales_meeting_transcriber`
5. `support_rag_eval_runner`
6. `contract_section_partitioner`

## Held Back

- `competitor_price_change_monitor`: real competitor price monitoring is deferred; mock/approved page snapshots only.
- `receipt_line_ocr_extractor`: wait for OCR model/dependency boundary review.
- `sales_speaker_timeline`: wait for diarization model/user-agreement review.
- `sales_followup_action_connector`: send to legal/security review; only dry-run payloads may be discussed before approval.

