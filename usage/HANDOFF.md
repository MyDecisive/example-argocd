# Handoff Guide – Operating the MyDecisive GitOps Repo

## What you’re operating
This repo manages **collector behavior** (solutions + overrides) and drives Argo CD to apply it.

## Argo CD Applications (layers)
1) **mdai-apps** (`argocd/argocd.yaml`)
   - App-of-apps bootstrap
   - Source: `argocd/apps/**` (directory recursion enabled)
2) **mdai** (`argocd/apps/mdai.yaml`)
   - Platform install (Helm)
   - Installs CRDs/controllers needed by MyDecisive CRs
3) **mdai-global-config** (`argocd/apps/mdai-global-config.yaml` or `mdai-addons.yaml`)
   - Global baseline config
   - Source: `argocd/mdai/live/global`
4) **mdai-team-<team>** (`argocd/apps/teams/*.yaml`)
   - Team bundle config
   - Source: `argocd/mdai/live/teams/<team>`

## Golden rule
**Argo CD syncs only from `argocd/mdai/live/**`.**

`catalog/` is a library and does nothing until referenced from `live/`.

## Repo layout (what goes where)
- `argocd/mdai/catalog/globals/base`  
  Minimal always-on baseline templates (hub + collector). Keep this small.
- `argocd/mdai/catalog/globals/contracts/*`  
  Shared contracts (variable keys/types, shared CRs required by a solution).
- `argocd/mdai/catalog/solutions/<use-case>/vN`  
  Reusable, parameterized solution bundles. If a solution needs “global” CRs, include them as `contracts/` inside the solution (solution-scoped globals).
- `argocd/mdai/catalog/teams/<team>`  
  Team params + overrides (regex/templates, hub name parameters, envFrom ordering).
- `argocd/mdai/live/global`  
  What is enabled globally (baseline and globally-on solutions).
- `argocd/mdai/live/teams/<team>`  
  What is enabled for a team (inherits baseline + selected solutions + team overlay).

## Making changes (safe workflow)
1) Edit the right layer (`catalog/` or `live/`)
2) Preview:
   ```bash
   kustomize build argocd/mdai/live/global > /tmp/global.yaml
   kustomize build argocd/mdai/live/teams/<team> > /tmp/team.yaml
   ```
3) Open PR and paste relevant `kustomize build` output/diff
4) After merge, sync the smallest-scope Argo app that matches the change:
   - global change → **mdai-global-config**
   - team-only change → **mdai-team-<team>**

## Common gotchas
- **Child apps not appearing:** check mdai-apps points at the correct `targetRevision` and path `argocd/apps` (with recursion).
- **Repo branch mismatch:** all Applications should use the same `targetRevision`.
- **Template rename breakage:** renaming the template hub/collector names requires updating Kustomize replacement selectors.

See also: [CHANGE_IMPACT_MATRIX.md](./CHANGE_IMPACT_MATRIX.md) and [ANTI_PATTERNS.md](./ANTI_PATTERNS.md).
