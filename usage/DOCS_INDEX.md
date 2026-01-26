# Documentation Index

Pick your path:

- **New engineer** → [ONBOARDING.md](./ONBOARDING.md)
- **Daily operator** → [CHEATSHEET.md](./CHEATSHEET.md)
- **Deep operations** → [HANDOFF.md](./HANDOFF.md)
- **Architecture / mental model** → [ARCHITECTURE.md](./ARCHITECTURE.md)
- **Security / governance** → [SECURITY_SIGNOFF_WORKFLOW.md](./SECURITY_SIGNOFF_WORKFLOW.md), [CHANGE_IMPACT_MATRIX.md](./CHANGE_IMPACT_MATRIX.md), [ANTI_PATTERNS.md](./ANTI_PATTERNS.md)

## Golden Rule
**Argo CD syncs only from `argocd/mdai/live/**`.**

## Argo CD Apps (in Argo UI)
- **mdai-apps** (bootstrap): reads `argocd/apps/**` and creates/manages child apps
- **mdai** (platform): installs MyDecisive/OTel mesh components and CRDs (Helm)
- **mdai-global-config** (global config): syncs `argocd/mdai/live/global`
- **mdai-team-<team>** (team config): syncs `argocd/mdai/live/teams/<team>`

## Repo model (TL;DR)
- `argocd/mdai/catalog/**` = definitions (inactive library)
- `argocd/mdai/live/**` = activation (what Argo syncs)

## Where to make changes
- Add/change a **solution** → `argocd/mdai/catalog/solutions/<use-case>/vN`
- Add/change a **contract** → `argocd/mdai/catalog/globals/contracts/<contract-name>`
- Team-specific override → `argocd/mdai/catalog/teams/<team>/patches`
- Enable/disable for team → `argocd/mdai/live/teams/<team>/kustomization.yaml`
