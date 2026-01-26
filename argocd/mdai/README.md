# MyDecisive Collector Mesh GitOps (Monorepo)

This repo is structured so that:
- `catalog/` is the source-of-truth library (globals, solution bundles, team overlays).
- `live/` is the only place Argo CD should sync from (what is actually enabled).

## Key paths
- Global enforced baseline: `argocd/mdai/live/global`
- Team-specific enablement: `argocd/mdai/live/teams/<team>`

## Add a new solution
1. Create `argocd/mdai/catalog/solutions/<solution-name>/vN/`
2. Put manifests or Kustomize patches there with a `kustomization.yaml`
3. Reference it from one or more team bundles under `argocd/mdai/live/teams/<team>/kustomization.yaml`
