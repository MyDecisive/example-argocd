Put team-specific patches here (selectors/targets, env var values, etc.).

## Example override

- `pii-overrides-configmap.yaml` shows a team-scoped override of the shared PII env vars (e.g. `EMAIL_REGEX`, `EMAIL_TEMPLATE`).
- `patch-collector-envfrom.yaml` wires the override ConfigMap into the collector by extending `spec.envFrom`.

Order matters: later `envFrom` entries override earlier keys when the same env var appears.
