# Solution-Gated Cluster-Scoped Resources

In this repository, **cluster-scoped does not mean always enabled**.

Some cluster-scoped resources (for example RBAC, contracts, or controller configuration)
are required **only when a specific solution is enabled**.
Their existence is therefore treated as an **environment decision**, not a platform default.

---

## How this is modeled in this repo

| Concern | Location | Purpose |
|------|----------|---------|
| Solution definitions | `argocd/mdai/catalog/solutions/<solution>/vN/` | Defines what a solution requires, including any cluster-scoped resources |
| Solution activation | `argocd/mdai/live/<env>/` | Chooses whether the solution exists in that environment |
| Always-on baseline | `argocd/mdai/catalog/globals/` | Minimal global resources required for all environments |

---

## When it is **OK** to gate cluster-scoped resources by solution

✅ **DO** use solution-gated cluster-scoped resources when **all** of the following are true:

| Condition | Explanation |
|---------|-------------|
| Needed only if the solution runs | The resource has no purpose if the solution is disabled |
| Clearly owned by one solution | Ownership is unambiguous (e.g., payments owns payments RBAC) |
| Safe to create/remove with enablement | Enabling or disabling the solution predictably adds/removes it |
| Suitable for dev-first enablement | You want to validate behavior in dev before promoting to prod |

**Common examples**
- `ClusterRole` / `ClusterRoleBinding` used only by a solution’s controller
- Cluster-scoped “contract” CRs that wire a solution into the platform
- Policy or admission configuration scoped narrowly to the solution

---

## When **NOT** to do this

🚫 **DON’T** gate cluster-scoped resources when any of the following apply:

| Anti-pattern | Why it’s a problem |
|-------------|--------------------|
| Platform prerequisite | Required for the platform to function at all |
| Affects unrelated workloads | Changes behavior outside the solution’s scope |
| Shared by multiple solutions | Ownership becomes unclear → belongs to the platform |
| Unsafe to remove | Disabling could break unrelated state or data |

These resources belong in the **global baseline**, not in solution definitions.

---

## Activation rule (repository invariant)

| State | Result |
|-----|--------|
| Solution referenced in `live/<env>/` | Cluster-scoped resources exist in that environment |
| Solution not referenced | Cluster-scoped resources do **not** exist |

> **Existence is an environment decision**, even for cluster-scoped resources.

---

## Rule of thumb

> **Cluster-scoped means “one per cluster *when enabled*,” not “always on.”**

---

## Why this pattern exists

- Keeps the global baseline minimal and auditable
- Avoids unnecessary cluster-wide permissions
- Makes solution enablement explicit and reviewable in Git
- Supports safe dev → prod promotion with clear intent
