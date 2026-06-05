# P1 Expanded Platform Callable Skills

This folder contains the first stable expansion batch promoted on 2026-06-06.

## Summary

- Promoted Skills: 42
- Stable registry total after promotion: 55
- Source: previously screened candidate draft L3 packages from Quality Sprint 01 to Quality Sprint 05
- Promotion rule: platform-base compatible, OpenAI-compatible relay/model-gateway compatible, useful for SMB six-department workflows, and packaged with `SKILL.md` plus `skill.yaml`

## Runtime Boundary

These Skills are platform-callable under controlled mock/read-only/dry-run boundaries. They do not grant permission to access real accounts, call real SaaS APIs, write business systems, send messages, upload files, or execute external actions unless a later controlled harness explicitly enables and audits that action.

Keep all per-Skill forbidden actions and manual-review triggers in each `SKILL.md` and `skill.yaml`.
