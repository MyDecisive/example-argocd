# GitOps Anti-Patterns (What NOT To Do)

These patterns increase risk and should be rejected in review.

## ❌ Editing catalog files without updating live/
Changes in `catalog/` do nothing unless referenced from `live/`.

## ❌ Copy/pasting full YAML into team folders
This defeats reuse. Prefer solutions + patches.

## ❌ Hardcoding team names in solutions
Solutions must be reusable. Team-specific values belong in `catalog/teams/`.

## ❌ Renaming template resources casually
Changing hub or collector template names breaks replacements.

## ❌ Using kubectl to 'hotfix'
Always fix via Git and let Argo reconcile.

## ❌ Mixing enforcement logic into globals
Globals are baselines, not hidden solutions.
