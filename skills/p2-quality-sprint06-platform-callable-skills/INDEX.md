# P2 Quality Sprint 06 Stable Skill Index

Updated: 2026-06-07

## Promotion Scope

- Batch count: 8
- Stable total after this batch: 63 = P0 13 + P1 42 + P2 8
- Source: Quality Sprint 06 L2 simulated pass, draft L3 packaging, and platform candidate acceptance pass
- These Skills can be installed or migrated as Agent Skills while preserving their mock/read-only/dry-run boundaries.

## Skill List

| # | Skill ID | Type |
| ---: | --- | --- |
| 1 | `sharepoint_policy_qc_readonly_agent` | read-only / knowledge-policy |
| 2 | `teams_handoff_digest_readonly_agent` | read-only / collaboration |
| 3 | `google_sheets_budget_variance_readonly_agent` | read-only / finance-reporting |
| 4 | `zendesk_answerbot_deflection_readonly_agent` | read-only / support |
| 5 | `hubspot_deal_handoff_quality_dryrun_agent` | dry-run / sales |
| 6 | `stripe_failed_payment_recovery_draft_readonly_agent` | read-only / finance-renewal |
| 7 | `bamboohr_timeoff_coverage_readonly_agent` | read-only / HR |
| 8 | `gmail_label_health_readonly_agent` | read-only / email-ops |

## Shared Boundary

- No API keys, `.env`, real customer files, or provider tokens in the repo.
- No real account access in the packaged Skill.
- No email, Teams, CRM, HR, finance, payment, subscription, inventory, document, label, task, or file-share writes.
- No sending, uploading, refunds, compensation, payment capture, inventory update, subscription update, ledger write, tax filing, task creation, or file sharing.
- Real Harness smoke is still required before production SaaS/API use.
