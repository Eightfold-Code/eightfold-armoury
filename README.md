# Eightfold Armoury

**The visual registry for Eightfold Harness.**

Eightfold Armoury is the presentation layer for [Eightfold Harness](https://github.com/Eightfold-Code/eightfold-harness). It publishes small, auditable **skins** and **themes** that change how Harness looks and feels without changing what it can do.

Armoury is deliberately separate from [Eightfold Treasury](https://github.com/Eightfold-Code/eightfold-treasury):

- **Harness** is the engine and runtime.
- **Treasury** is the catalog of capabilities and adaptations.
- **Armoury** is the catalog of visual skins and themes.

> Armoury is a developer preview. The manifest and registry formats may evolve while the Eightfold interfaces are stabilized.

## Using Armoury

Armoury is consumed through the Eightfold Harness CLI:

```bash
pnpm dsh eightfold armoury list
pnpm dsh eightfold armoury search dark

pnpm dsh eightfold skin add obsidian
pnpm dsh eightfold skin use obsidian
pnpm dsh eightfold skin update obsidian
pnpm dsh eightfold skin remove obsidian
```

A profile can select a skin independently from its installed adaptations:

```json
{
  "adaptations": ["session-search", "developer-tools"],
  "skin": "obsidian"
}
```

A skin is presentation-only. It may define design tokens, typography, component variants, icons, and layout preferences. It must not ship executable behavior or alter permissions.

## Repository model

### `main`

The default branch contains the public catalog and the format documentation:

```text
eightfold-armoury/
├── README.md
├── registry.json
├── schemas/
│   ├── registry.schema.json
│   └── skin.schema.json
└── docs/
    ├── architecture.md
    └── publishing.md
```

### `skin/<name>`

Each published skin has its own branch, for example:

```text
skin/obsidian
```

The tip of a skin branch contains only the skin package:

```text
/
├── eightfold.skin.json
├── README.md
├── theme.json
├── tokens/
│   ├── colors.json
│   └── typography.json
└── presets/
    └── default.json
```

Registry entries pin a full Git commit SHA so installs remain reproducible even when a development branch moves.

## Installation flow

When Harness installs a skin, it:

1. Fetches and validates `registry.json`.
2. Resolves the selected skin and its pinned source commit.
3. Downloads the archive for that exact snapshot.
4. Validates the root `eightfold.skin.json` manifest.
5. Validates token and preset paths.
6. Extracts the skin into the local Armoury state directory.
7. Records the installed skin ID, version, and source commit.

This keeps skins small, auditable, and independently removable.

## Design goals

- **Visual isolation** — skin packages contain presentation data only.
- **Small installs** — fetch one skin snapshot instead of cloning the catalog.
- **Reproducibility** — registry entries resolve to exact Git commits.
- **Composable profiles** — visual selection stays independent from capabilities.
- **Simple discovery** — one registry exposes skins and optional collections.
- **Native Harness integration** — Armoury does not become a second plugin runtime.

## Related projects

- [Eightfold Harness](https://github.com/Eightfold-Code/eightfold-harness) — the runtime.
- [Eightfold Treasury](https://github.com/Eightfold-Code/eightfold-treasury) — the capability registry.
