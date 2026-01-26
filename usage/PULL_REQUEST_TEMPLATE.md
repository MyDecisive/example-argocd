# Change Summary
<!-- What is being changed and why? -->

## Type of Change
- [ ] Global baseline
- [ ] Solution (new or update)
- [ ] Team enablement
- [ ] Team override only

## Affected Teams
<!-- List team names or 'global' -->

## Enabled / Disabled Solutions
<!-- e.g. pii-redaction v1 enabled for team-payments -->

## Kustomize Output (required)
Paste the output (or diff) of:
```bash
kustomize build argocd/mdai/live/teams/<team>
```

## Validation Checklist
- [ ] `kustomize build` succeeds
- [ ] Hub name resolves correctly
- [ ] Collector label `mydecisive.ai/hub-name` matches hub
- [ ] No unintended teams affected

## Rollback Plan
<!-- How to undo if this causes issues -->

## Which Argo app should be synced?
- [ ] mdai (platform)
- [ ] mdai-global-config (global)
- [ ] mdai-team-<team> (team)
