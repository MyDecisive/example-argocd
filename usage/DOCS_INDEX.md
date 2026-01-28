# Documentation Index

Pick your path:

Start here:
- [HOW_TO_READ_DOCS.md](./HOW_TO_READ_DOCS.md)
- [REPO_STRUCTURE.md](./REPO_STRUCTURE.md)
- **New engineer** → [ONBOARDING.md](./ONBOARDING.md)
- **Daily operator** → [CHEATSHEET.md](./CHEATSHEET.md)
- **Deep operations** → [HANDOFF.md](./HANDOFF.md)
- **Architecture / mental model** → [ARCHITECTURE.md](./ARCHITECTURE.md)
- **Security / governance** → [SECURITY_SIGNOFF_WORKFLOW.md](./SECURITY_SIGNOFF_WORKFLOW.md), [CHANGE_IMPACT_MATRIX.md](./CHANGE_IMPACT_MATRIX.md), [ANTI_PATTERNS.md](./ANTI_PATTERNS.md)


Other helpful docs:
- [Solution-gated, cluster-scoped resources](./SOLUTION_GATED_CLUSTER_SCOPED_RESOURCES.md)

## Release

- **Production release checklist** → [RELEASE_CHECKLIST_PROD.md](./RELEASE_CHECKLIST_PROD.md)

## Diagrams
- **Repo + Argo CD flow diagram** → [DIAGRAMS.md](./diagrams/DIAGRAMS.md)

## Repo model (TL;DR)

| Path                     | Role            | Description                                                                                               |
| ------------------------ | --------------- | --------------------------------------------------------------------------------------------------------- |
| `argocd/mdai/catalog/**` | **Definitions** | Reusable, inactive library of solutions, contracts, and bases. Never synced directly by Argo CD.          |
| `argocd/mdai/live/**`    | **Activation**  | Environment-specific activation of definitions. These paths are the only ones Argo CD is allowed to sync. |

## Where to make changes
| What you want to do         | Path to change                                           | Notes                                         |
| --------------------------- | -------------------------------------------------------- | --------------------------------------------- |
| Add / modify a **solution** | `argocd/mdai/catalog/solutions/<use-case>/vN`            | Versioned, reusable, not environment-specific |
| Add / modify a **contract** | `argocd/mdai/catalog/globals/contracts/<contract-name>`  | Cluster-scoped or shared primitives           |
| Team-specific override      | `argocd/mdai/live/<env>/teams/<team>/patches`            | Environment-specific patches only             |
| Enable / disable for a team | `argocd/mdai/live/<env>/teams/<team>/kustomization.yaml` | Controls activation per environment           |
| Architecture diagrams | `usage/diagrams/` | PNG for viewing, draw.io sources for editing |
