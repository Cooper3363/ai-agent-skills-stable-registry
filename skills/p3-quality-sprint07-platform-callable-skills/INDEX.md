# P3 Quality Sprint 07 Stable Skill Index

Updated: 2026-06-07

## Promotion Scope

- Batch count: 8
- Stable total after this batch: 71 = P0 13 + P1 42 + P2 8 + P3 8
- Source: Quality Sprint 07 L2 simulated pass, draft L3 packaging, and platform candidate acceptance accepted
- These Skills can be installed or migrated as Agent Skills while preserving their mock/read-only boundaries.

## Skill List

| # | Skill ID | Type |
| ---: | --- | --- |
| 1 | `intercom_article_decay_readonly_agent` | read-only / support-knowledge |
| 2 | `shopify_return_product_quality_readonly_agent` | read-only / ecommerce-aftersales |
| 3 | `metabase_executive_digest_readonly_agent` | read-only / executive-metrics |
| 4 | `docusign_missing_signature_readonly_agent` | read-only / contract-ops |
| 5 | `mailchimp_deliverability_qc_readonly_agent` | read-only / email-marketing |
| 6 | `helpscout_saved_reply_gap_readonly_agent` | read-only / support-macro |
| 7 | `front_account_handoff_readonly_agent` | read-only / customer-handoff |
| 8 | `amplitude_activation_dropoff_readonly_agent` | read-only / product-analytics |

## Shared Boundary

- No API keys, `.env`, real customer files, or provider tokens in the repo.
- No real account access in the packaged Skill.
- No support, ecommerce, analytics, contract, marketing, CRM, inbox, product analytics, inventory, subscription, finance, document, task, or file-share writes.
- No sending, uploading, refunds, compensation, payment capture, inventory update, subscription update, ledger write, tax filing, task creation, or file sharing.
- Real Harness smoke is still required before production SaaS/API use.
