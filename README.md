# AI Agent Skills Stable Registry

This repository is the stable delivery registry for customer-callable AI.Skills.

## Current Stable Count

- Customer-callable Skill count: 71
- P0 baseline count: 13
- P1 expanded count: 42
- P2 Quality Sprint 06 count: 8
- P3 Quality Sprint 07 count: 8
- P0 baseline path: `skills/p0-first-13-platform-callable-skills`
- P1 expanded path: `skills/p1-expanded-platform-callable-skills`
- P2 Quality Sprint 06 path: `skills/p2-quality-sprint06-platform-callable-skills`
- P3 Quality Sprint 07 path: `skills/p3-quality-sprint07-platform-callable-skills`

## Promotion Rule

The 2026-06-06 promotion rule is: a Skill or Agent can enter this stable registry if it has a complete `SKILL.md` and `skill.yaml`, fits the platform base, can use the OpenAI-compatible relay/model gateway, and serves SMB six-department scenarios.

P1/P2/P3 expanded Skills keep strict runtime boundaries: mock/read-only/dry-run by default, no real account access, no key in repo, no sending, no upload, no business-system writes, and no external action unless a later platform harness explicitly enables it.

Do not add API keys, `.env` files, real customer files, or generated media outputs.
