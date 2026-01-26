# Architecture & Mental Model

## Flow of control
- `catalog/` contains **definitions**
- `live/` contains **activation**
- Argo CD syncs only `live/`

## Layers in Argo CD
- **mdai-apps** bootstraps child apps from `argocd/apps/**`
- **mdai** installs the platform (Helm, CRDs/controllers)
- **mdai-global-config** applies `argocd/mdai/live/global`
- **mdai-team-<team>** applies `argocd/mdai/live/teams/<team>`

## Global vs Solution-scoped “global” CRs
Some CRs are cluster-scoped (global), but should only exist when a **solution** is enabled.
Pattern:
- Keep `globals/base` minimal (always-on only)
- Put solution-required cluster-scoped CRs under `catalog/solutions/<solution>/vN/contracts`
- Enable the solution in `live/` to activate its contracts

## Why this scales
- Adding solutions does not impact teams until enabled
- Team overrides are data-only and isolated
- Reviews are small because `live/` shows activation explicitly
