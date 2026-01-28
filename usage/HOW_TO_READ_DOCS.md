# How to Read These Docs

This repo intentionally separates documentation by concern so it stays consistent over time.

## Recommended reading order

1) **REPO_STRUCTURE.md**
   - What exists in the repo and why
   - Directory intent and the repo “contract” (invariants)

2) **ARCHITECTURE.md**
   - How the system behaves at runtime
   - Argo CD layering, flow of control, and scaling rationale

3) **The rest of `usage/`**
   - Step-by-step operational workflows (onboarding, hands-on lab, release checklist, handoff)

## Rule of thumb

- **Structure** answers *what exists and where*
- **Architecture** answers *how it works and why*
- **Usage** answers *how to operate it safely*

If you feel tempted to copy/paste definitions of `catalog/` vs `live/` into a workflow doc, link to
**REPO_STRUCTURE.md** instead.
