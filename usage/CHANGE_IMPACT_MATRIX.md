# Change Impact Matrix – MyDecisive GitOps

This document explains **who is impacted** by different kinds of changes.

| Change Location | Examples | Impact Scope | Review Required |
|----------------|----------|--------------|-----------------|
| catalog/globals/base | hub.yaml, otel.yaml | ALL teams | Platform + SecOps |
| catalog/globals/contracts | PII variable schema | Teams using contract | Platform + SecOps |
| catalog/solutions | New or updated solution | Opt-in teams only | Platform |
| live/<env>/teams/<team>/patches | Team-specific overrides (patches/configmaps) | Single team | Platform |
| live/<env>/global | Enable global baseline | ALL teams | Platform + SecOps |
| live/<env>/teams/<team> | Enable/disable solution | Single team | Platform |
| chart/ | Operator / CRDs | Cluster-wide | Infra + SecOps |

## Rule of Thumb
If it touches `globals/` or `live/<env>/global`, assume **cluster-wide impact**.
