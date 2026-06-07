# P2 Quality Sprint 06 Platform Callable Skills

This folder contains the second stable expansion batch promoted on 2026-06-07.

## Summary

- Promoted Skills: 8
- Stable registry total after promotion: 63
- Source: Quality Sprint 06 draft L3 packages with platform candidate acceptance passed
- Promotion rule: platform-base compatible, OpenAI-compatible relay/model-gateway compatible, useful for SMB six-department workflows, and packaged with `SKILL.md` plus `skill.yaml`

## Runtime Boundary

These Skills are platform-callable under controlled mock/read-only/dry-run boundaries. They do not grant permission to access real accounts, call real SaaS APIs, write business systems, send messages, upload files, or execute external actions unless a later controlled harness explicitly enables and audits that action.

Real Harness smoke remains required before production SaaS/API use. Keep all per-Skill forbidden actions, minimum read-only scopes, dry-run payload schemas, audit-log schemas, fallback rules, and manual-review triggers in each `SKILL.md` and `skill.yaml`.
