# One-Page Cheat Sheet

## The Golden Rule
**Argo CD syncs only from `argocd/mdai/live/**`.**

## Which Argo app do I sync?
- Platform broken / CRDs missing → **mdai**
- Global baseline change → **mdai-global-config**
- One team change → **mdai-team-<team>**
- Argo not creating child apps → **mdai-apps**

## Where do I edit?
| Task | File/Folder |
|---|---|
| Change platform install (Helm values) | `argocd/apps/mdai.yaml` + `argocd/mdai/chart/` |
| Change global baseline | `argocd/mdai/live/global/` (references catalog) |
| Enable/disable solution for a team | `argocd/mdai/live/teams/<team>/kustomization.yaml` |
| Change solution behavior | `argocd/mdai/catalog/solutions/<use-case>/vN/` |
| Change shared contract (e.g., variables schema) | `argocd/mdai/catalog/globals/contracts/<name>/` |
| Team override (regex/template) | `argocd/mdai/catalog/teams/<team>/patches/` |
| Add a new team app | `argocd/apps/teams/<team>.yaml` |

## Preview what Argo will apply
```bash
kustomize build argocd/mdai/live/global
kustomize build argocd/mdai/live/teams/<team>
```

## Add a new team (fast)
1) Copy a team overlay + live bundle:
```bash
cp -r argocd/mdai/catalog/teams/team-example argocd/mdai/catalog/teams/team-new
cp -r argocd/mdai/live/teams/team-example argocd/mdai/live/teams/team-new
```
2) Set `HUB_NAME` + `HUB_VARS_CM` in the team params ConfigMap.
3) Add the team Argo Application: `argocd/apps/teams/mdai-team-new.yaml`.

## Common gotcha
All apps should point at the **same `targetRevision`** (same branch/tag) so Argo sees the same repo state.
