# MyDecisive Collector Mesh – Repo Usage (Operator + Contributor Guide)

This repository manages **collector behavior and governance** for the MyDecisive/OpenTelemetry collector mesh using Argo CD + GitOps.

## What this repo controls
- Use-case “solutions” (PII redaction, data filtration, routing, etc.)
- Global baselines and shared “contracts” (variable schemas, shared CRs required by a solution)
- Team-specific overrides (regex/templates, parameter values)

## What this repo does NOT control
- Application workloads
- Secrets (store/consume references only)
- One-off manual cluster hotfixes (avoid `kubectl apply`)

## Golden Rule
**Argo CD syncs only from `argocd/mdai/live/**`.**  
Everything under `argocd/mdai/catalog/**` is a library and has no effect unless referenced from `live/`.

## Argo CD Applications (what you’ll see in Argo UI)
- **mdai-apps**: app-of-apps bootstrap. Creates the other Applications from `argocd/apps/**`.
- **mdai**: platform install (Helm). Installs operator/controllers/CRDs.
- **mdai-global-config**: global behavior baseline. Syncs `argocd/mdai/live/global`.
- **mdai-team-<team>** (e.g., mdai-team-payments): team bundle. Syncs `argocd/mdai/live/teams/<team>`.

Start at `DOCS_INDEX.md` for the full map.
