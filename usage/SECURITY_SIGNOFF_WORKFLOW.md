# Security Sign-off Workflow – MyDecisive GitOps

This workflow defines when and how security approval is required.

## Changes Requiring Security Review
- Modifications under `catalog/globals/`
- Any change to `live/global`
- Changes affecting PII handling
- Operator / CRD changes

## Standard Flow
1. Engineer opens PR
2. PR includes:
   - change summary
   - kustomize build output
3. Platform review
4. Security review (if required)
5. Merge
6. Argo sync

## Fast Path (No Security Review)
- Team-only overrides
- Enabling existing solutions for one team
- Non-functional documentation changes

## Emergency Rollback
- Remove solution reference from `live/teams/<team>`
- Or suspend Argo Application
