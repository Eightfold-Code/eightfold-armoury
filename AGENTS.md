# Contributing to Eightfold Armoury

Armoury is the visual distribution layer for Eightfold Harness. Keep this repository focused on presentation data and reproducible skin packages.

## Branch rules

- `main` contains the registry, schemas, and documentation only.
- Every skin is published from `skin/<id>`.
- Skin packages must be presentation-only. Do not add executable plugins, network clients, credentials, or permission-changing logic.
- Registry entries must pin a full 40-character commit SHA.
- Skin IDs and branch names use lowercase kebab case.

## Before publishing

1. Validate `eightfold.skin.json` against `schemas/skin.schema.json`.
2. Validate `registry.json` against `schemas/registry.schema.json`.
3. Confirm that all paths referenced by the manifest exist in the skin snapshot.
4. Review the diff for secrets and executable code.
5. Update the registry only after the skin commit is pushed.

The format is a developer preview and may evolve alongside Eightfold Harness.
