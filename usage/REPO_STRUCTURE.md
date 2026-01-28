# Repository Structure & Intent

This document defines the **static structure and rules** of the repository. It is the source of truth
for directory intent and “what goes where”.

```text
repo/
├─ argocd/
│  ├─ argocd.yaml                # Dev app-of-apps (enabled by default)
│  ├─ argocd-prod.yaml           # Prod app-of-apps (opt-in)
│  ├─ apps/
│  │  ├─ dev/                    # Child Applications (auto-sync)
│  │  └─ prod/                   # Child Applications (manual sync / safe mode)
│  └─ mdai/
│     ├─ catalog/                # Reusable definitions (never deployed directly)
│     ├─ live/
│     │  ├─ dev/                 # Deployable state for dev (auto-sync)
│     │  │  ├─ global/
│     │  │  └─ teams/<team>/
│     │  └─ prod/                # Deployable state for prod (manual sync)
│     │     ├─ global/
│     │     └─ teams/<team>/
│     └─ chart/                  # Platform Helm chart + values (controllers/CRDs/etc)
├─ terraform/                    # Infra provisioning (VPC/EKS/addons)
└─ usage/                        # Operator + contributor docs
```

## Directory intent


This document describes **what goes where** in the repository and the **invariants** that keep the deployment model safe and predictable.

---

### Repository layout (mental model)

#### Core paths & responsibilities

| Path | Role | Key rules |
|----|----|----|
| `argocd/mdai/catalog/` | **Reusable definitions** | Never deployed directly. Safe to change without activating environments. |
| `argocd/mdai/live/<env>/` | **Deployable state (activation)** | Only paths Argo CD may sync. Encodes environment intent. |
| `argocd/apps/<env>/` | **Argo CD Applications** | App-of-apps bootstraps child apps per environment. |
| `argocd/mdai/chart/` | **Platform install** | Helm chart for CRDs/controllers. Separate from workloads. |

---

### `argocd/mdai/catalog/` — reusable definitions

**What lives here**
- Bases and shared overlays
- Solution definitions and contracts
- No environment-specific intent

**Rules**
- ❌ Never targeted by Argo CD Applications
- ✅ Safe to modify without triggering a rollout

> **Mental model:** `catalog/` is a **library**, not an environment.

---

### `argocd/mdai/live/<env>/` — deployable state (activation)

**What lives here**
- Environment-specific enablement
- References to `catalog/`
- Environment- and team-specific patches

**Rules**
- ✅ Only deployable manifests
- ✅ Changes represent explicit environment decisions

---

### `argocd/apps/<env>/` — Argo CD Application layer

| Environment | Behavior |
|------------|----------|
| `dev/` | Enabled by default, auto-sync |
| `prod/` | Opt-in, manual sync (“safe mode”) |

**Purpose**
- App-of-apps bootstraps child Applications
- Controls **how** Argo CD deploys, not **what** is deployed

---

### `argocd/mdai/chart/` — platform install

**What lives here**
- Helm chart and values for:
  - CRDs
  - controllers
  - platform operators

**Rule**
- Platform installation is **separate** from workload configuration in `live/`

---

### Repository contract (invariants)

| Invariant | Why it matters |
|---------|----------------|
| Argo CD must never target `argocd/mdai/catalog/` | Prevents accidental activation |
| Only `argocd/mdai/live/<env>/` is deployable | Keeps environment intent explicit |
| Dev auto-syncs by default | Fast feedback |
| Prod is manual by default | Safety and control |
| Rollbacks are done via Git + Argo sync | Deterministic recovery |

> Violating these rules is considered a **repository bug**, not user error.


Violating these rules is considered a repository bug.
