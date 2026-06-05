# P0 first 13 platform smoke test plan

Date: 2026-06-03

Scope: This is the platform access smoke checklist for the 13 formal Skills in this package. All tests can start with mock input for schema validation. Before production use, each Skill still needs a real Harness smoke test through the platform wrapper.

Out of scope: no external web access, no real OCR, no real PDF parsing, no scraping, no dependency installation, no automatic publishing, no CRM writeback, no tax/reimbursement/audit conclusion.

## Summary

| Group | Count | Note |
| --- | ---: | --- |
| Formal Skills | 13 | Included in this package |
| Component pool | 3 | Not counted as formal Skills |
| Mock-first smoke tests | 13 | All 13 can use mock inputs first |
| Real Harness smoke tests before launch | 13 | Required before production handoff |

## Checklist

| Skill ID | Smoke test goal | Minimal Chinese input | Expected fields | Failure criteria | Human review trigger | Real dependency needed |
| --- | --- | --- | --- | --- | --- | --- |
| `marketing_copy_pack` | Generate a Chinese marketing copy pack with compliance notes | Yoga studio, 9.9 trial class, Xiaohongshu, booking goal | `headlines`, `body_copy`, `ctas`, `compliance_notes`, `revision_suggestions` | Missing core fields; medical or absolute claims; no CTA | regulated industry, absolute claims, medical claims, price sensitive | Mock first; real platform wrapper before launch |
| `daily_weekly_metrics_reporter` | Generate daily/weekly report structure | Weekly report with revenue and order metrics | `executive_summary`, `key_metrics`, `anomalies`, `likely_factors`, `recommended_actions`, `data_quality_notes` | Drops metrics; treats suggestion as certain business conclusion; no data quality note | unclear metric definition, severe missing data, major exception | Mock first; real schema and numeric handling before launch |
| `metric_exception_classifier` | Classify metric exceptions and review steps | Delivery orders 420 vs 610, drop thresholds 20/35 | `exception_detected`, `severity`, `exception_type`, `possible_causes`, `verification_steps`, `human_review_required`, `confidence` | Wrong severity; certain causation; no verification steps | critical exception, cashflow impact, system issue, low confidence high impact | Mock first; real threshold handling before launch |
| `faq_answer_with_citations` | Answer from mock knowledge base with true citations | "How can members request an invoice?" plus mock KB ref | `answer`, `citations`, `confidence`, `handoff_required`, `handoff_reason`, `risk_flags` | Fabricated citation; unsupported answer; low confidence without handoff | low confidence, no citation, complaint, refund, account recovery | Mock KB first; real wrapper/KB before launch |
| `support_ticket_classifier` | Classify support tickets with priority and handoff | "My account may be stolen and I cannot receive SMS code" | `category`, `subcategory`, `priority`, `risk_flags`, `suggested_owner`, `human_review_required`, `confidence` | Account security classified as normal login; priority too low | account security, privacy sensitive, high priority, low confidence | Mock first; enum validation before launch |
| `structured_campaign_brief` | Convert raw campaign need into structured brief | Fresh food weekend promo for WeChat group and public account | `campaign_summary`, `target_audience`, `key_messages`, `creative_requirements`, `placement_variables`, `missing_fields`, `human_review_required` | Missing creative requirements; fabricated inventory; price error | price unclear, inventory unclear, regulated industry | Mock first; placement variable validation before launch |
| `structured_metric_summary` | Summarize one metric conservatively | Restaurant revenue 52000 vs 68000 | `metric_summary`, `change_description`, `anomaly_flag`, `anomaly_reasoning`, `possible_factors`, `recommended_actions`, `data_quality_notes`, `human_review_required` | Wrong calculation; no anomaly flag; over-certain causation | financial decision, cashflow critical, unclear metric definition | Mock first; real calculation/schema before launch |
| `crm_note_structurer` | Structure CRM follow-up notes | Trial 3 days, likes reminders, says annual fee is expensive, follow up next Tuesday | `summary`, `customer_needs`, `objections`, `lead_stage`, `lead_score`, `next_actions`, `missing_info`, `human_review_required` | Fabricates customer facts; misses price objection; no next action | discount, contract terms, complaint, personal information | Mock first; CRM mapping before launch |
| `support_reply_guardrail` | Check unsafe support replies and suggest conservative rewrite | Customer asks refund, draft reply says immediate refund and compensation | `passed`, `risk_level`, `violations`, `suggested_revision`, `human_review_required`, `review_reason` | Fails to catch refund/compensation promise; no handoff | refund commitment, complaint escalation, account security | Mock first; ruleset validation before launch |
| `marketing_compliance_guard` | Check marketing copy compliance risks | Education ad: "guaranteed score improvement in 30 days" | `passed`, `risk_level`, `risk_items`, `suggested_rewrites`, `human_review_required`, `disclaimer` | Misses guarantee claim; risk too low; no rewrite | education result claims, absolute claims, sensitive audience | Mock first; industry ruleset before launch |
| `support_pii_redactor` | Validate Chinese PII redaction rules | Name, phone, ID number, detailed address | `redacted_text`, `entities`, `risk_level`, `manual_review_required`, `notes` | Phone/ID/address leakage; missing entity type; original text exposed | high sensitive PII, account recovery, redaction conflict | Mock first; local rule smoke before launch |
| `support_kb_doc_cleaner` | Clean and chunk mock KB text | Mock HTML FAQ with nav and footer | `cleaned_chunks`, `removed_noise`, `detected_sections`, `quality_warnings`, `manual_review_required` | Nav/footer kept; FAQ lost; no source metadata | unknown source, mixed sections, missing citation source | Mock/local text only; no real PDF/OCR in first smoke |
| `expense_invoice_parser` | Extract fields from mock invoice/OCR text | VAT invoice text with invoice number, date, seller, buyer, amount and tax | `invoice_type`, `seller_name`, `buyer_name`, `invoice_number`, `invoice_date`, `total_amount`, `tax_amount`, `currency`, `line_items`, `confidence`, `missing_fields`, `manual_review_required`, `not_tax_advice` | Wrong amount/tax/invoice number; missing `not_tax_advice`; no review on mismatch | amount mismatch, missing invoice number, low confidence, tax judgement | Mock/local OCR text only; no OCR/upload/tax decision |

## Component pool excluded from formal 13

| Component ID | Current position |
| --- | --- |
| `seo_keyword_extractor` | Marketing keyword candidate component |
| `data_quality_rules_check` | Data quality pre-check component |
| `partner_page_snapshot` | Mock/approved URL snapshot component only |

