# Next Top 10 L2 Chinese business test plan

Date: 2026-06-03

This file records the L2 mock/simulated test template for the next Top 10 candidates. It is not an L2 pass/fail result. Formal L2 testing starts only after license and commercial risk review.

## Boundaries

- No dependency installation.
- No external network access.
- No real files, real OCR, real audio, real PDF parsing, real web scraping, real OAuth, real CRM/email/calendar actions.
- High-risk candidates must start with mock input and dry-run output only.

## Summary Table

| Candidate | Skill ID | Test position | 3 Chinese mock cases | Expected output fields | Failure criteria | Permission boundary | Wait for license review |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DeepEval | `support_response_eval_suite` | Support response quality/safety evaluation suite | 1. FAQ correct answer with citation; 2. refund complaint with over-commitment; 3. account security reply gives bypass method | `overall_score`, `criteria_scores`, `failed_checks`, `risk_flags`, `suggested_fixes`, `human_review_required` | Misses refund/account security risk; does not separate factual and tone issues; no fix suggestion | Evaluate mock replies only; no KB/web access; no automatic production rewrite | Yes |
| Microsoft Presidio | `hr_resume_privacy_masker` | HR resume privacy redaction component | 1. resume with name/phone/email; 2. ID/address/birth date; 3. recommender phone and school/company info that must be distinguished | `redacted_text`, `entities`, `preserved_fields`, `risk_level`, `masking_notes`, `manual_review_required` | Misses high-sensitive PII; over-deletes work experience; unstable entity types | Mock resume text only; no real resume files; no external upload | Yes |
| Crawlee | `competitor_price_change_monitor` | Competitor price change monitor, mock HTML only | 1. old price 199 new price 159; 2. member price/list price mixed; 3. out-of-stock/offline page not comparable | `product_name`, `old_price`, `new_price`, `change_pct`, `change_type`, `evidence_snippets`, `risk_notes`, `manual_review_required` | Treats list price as current price; out-of-stock as price drop; no evidence snippet | Mock HTML/snapshot text only; no real scraping, proxy, login, robots/ToS bypass | Yes |
| Camelot | `quote_pdf_table_extractor` | Quote PDF table extraction, mock table text only | 1. standard quote table; 2. repeated header across pages; 3. total does not match line items | `quote_id`, `vendor`, `table_rows`, `subtotal`, `tax`, `total`, `confidence`, `missing_fields`, `validation_flags` | Column shift; duplicated page header; amount mismatch not flagged | Mock PDF table text or extracted rows only; no real PDF; no Camelot install | Yes |
| PaddleOCR | `receipt_line_ocr_extractor` | Receipt line item OCR extraction, mock OCR text only | 1. restaurant receipt with full line items; 2. blurry numbers; 3. discount/tax/mixed charges | `merchant`, `receipt_date`, `line_items`, `total_amount`, `discounts`, `tax_amount`, `confidence`, `ocr_quality_notes`, `manual_review_required` | Total mismatch; low-quality OCR not flagged; discount treated as item | Mock OCR text only; no real OCR, invoice upload, or cloud service | Yes |
| Whisper | `sales_meeting_transcriber` | Sales meeting transcription, mock transcript only | 1. sales visit with needs/objections/next steps; 2. noisy uncertain segments; 3. price/contract sensitive info | `transcript`, `segments`, `summary`, `customer_needs`, `objections`, `action_items`, `quality_notes`, `human_review_required` | Misses key actions; no quality warning; price commitment not flagged | Mock transcript and audio metadata only; no real audio handling or upload | Yes |
| WhisperX | `sales_speaker_timeline` | Sales meeting speaker timeline, mock diarized segments only | 1. two-speaker sales/customer call; 2. multi-person meeting; 3. overlapping or unknown speaker segments | `speaker_timeline`, `speaker_labels`, `key_moments`, `action_items_by_speaker`, `diarization_quality`, `manual_review_required` | Wrong speaker attribution; no uncertainty on overlap; action assigned to wrong speaker | Mock diarized segments only; no real audio/alignment/diarization | Yes |
| Ragas | `support_rag_eval_runner` | Support RAG answer evaluation component | 1. faithful answer with KB citation; 2. fabricated refund rule; 3. irrelevant context with confident answer | `faithfulness_score`, `answer_relevance`, `context_precision`, `citation_coverage`, `failure_reasons`, `eval_summary` | Hallucination not penalized; no context still high score; citation coverage wrong | Mock question/context/answer only; no real vector DB or web access | Yes |
| Unstructured | `contract_section_partitioner` | Contract clause partitioning, mock contract text only | 1. service contract with payment/term/SLA; 2. privacy/confidentiality clauses; 3. auto-renewal/breach/dispute clauses | `sections`, `section_types`, `clause_summaries`, `risky_sections`, `missing_sections`, `quality_warnings`, `manual_review_required` | Mixed clauses; misses auto-renewal/breach; summary treated as legal advice | Mock contract text only; no real contract file; no legal conclusion | Yes |
| Composio | `sales_followup_action_connector` | Sales follow-up action connector dry-run | 1. create CRM follow-up task; 2. generate but do not send email draft; 3. calendar invite payload check | `dry_run_actions`, `connector`, `target_system`, `payload`, `permission_required`, `safety_checks`, `execution_status`, `human_review_required` | Real send/write; missing permission warning; wrong recipient or privacy leak in payload | Dry-run only; no OAuth, CRM write, email send, or calendar event creation | Yes |

## Mock Input Rules

- Crawlee: use `mock_old_html` + `mock_new_html` or `old_snapshot` / `new_snapshot`; do not pass real URLs.
- Camelot: use `mock_pdf_table_text` or `mock_extracted_rows`; do not read PDFs.
- PaddleOCR: use `mock_ocr_text`; do not call OCR.
- Whisper: use `mock_transcript_text` + `audio_quality_notes`; do not process audio.
- WhisperX: use `mock_diarized_segments`; do not perform real speaker recognition.
- Composio: use `desired_action` + `crm_context` + `dry_run=true`; generate payload only, do not execute.

## Suggested Order After License Review

1. Low-permission mock L2 first: DeepEval, Ragas, Presidio, Unstructured.
2. Medium/high-boundary mock L2 next: Crawlee, Camelot, PaddleOCR, Whisper/WhisperX, Composio.
3. Real dependency testing only after separate approval for data, files, audio, web, OAuth, robots/ToS, and security boundaries.

