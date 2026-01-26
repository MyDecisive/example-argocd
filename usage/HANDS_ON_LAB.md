# Hands-On Lab – 10 Minutes

## Goal
Enable a solution for a fake team and preview output safely.

## Step 1 – Create a Team
Copy:
- catalog/teams/team-example → team-lab
- live/teams/team-example → team-lab

Set:
- HUB_NAME=mdaihub-team-lab
- HUB_VARS_CM=mdaihub-team-lab-variables

## Step 2 – Enable a Solution
Edit:
live/teams/team-lab/kustomization.yaml

Add:
catalog/solutions/data-filtration/v1

## Step 3 – Preview
```bash
kustomize build argocd/mdai/live/teams/team-lab
```

Verify:
- Hub name is correct
- Collector label matches
- Solution is present

## Step 4 – Rollback
Remove the solution reference and rebuild.

## Success Criteria
- No YAML duplication
- No catalog files modified unnecessarily
- Output is predictable
