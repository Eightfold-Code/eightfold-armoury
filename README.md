# Eightfold Armoury

**A small, reproducible catalog for Eightfold Harness skins.**

Armoury is the presentation layer for [Eightfold Harness](https://github.com/Eightfold-Code/eightfold-harness). It publishes auditable skins and themes that change how Harness looks and feels without changing its capabilities.

Armoury stays separate from [Eightfold Treasury](https://github.com/Eightfold-Code/eightfold-treasury):

- **Harness** owns the runtime.
- **Treasury** distributes capabilities and adaptations.
- **Armoury** distributes visual presentation.

> Armoury is in developer preview. The manifest and registry formats may change.

## Current catalog

The first published skin is [obsidian](skin/obsidian), a dark, high-contrast theme for focused Harness sessions.

The name refers to the visual style only. It is not the [Obsidian.md](https://obsidian.md/) application, integration, or dependency.

The live source of truth is the [registry](registry.json).

## Use Armoury through Harness

~~~bash
pnpm dsh eightfold armoury list
pnpm dsh eightfold armoury search dark
pnpm dsh eightfold skin add obsidian
pnpm dsh eightfold skin use obsidian
~~~

Update or remove a skin:

~~~bash
pnpm dsh eightfold skin update obsidian
pnpm dsh eightfold skin remove obsidian
~~~

A profile selects a skin independently from its installed adaptations:

~~~json
{
  "adaptations": ["session-search", "developer-tools"],
  "skin": "obsidian"
}
~~~

## Presentation-only by design

A skin may define:

- Design tokens and color palettes.
- Typography and density settings.
- Component variants and layout preferences.
- Icons and other presentation assets.

A skin must not ship executable behavior, network access, credentials, permission logic, or capability plugins. Functional extensions belong in Treasury.

## Repository model

### main

main contains the public catalog, schemas, and publishing documentation:

~~~text
eightfold-armoury/
├── registry.json
├── schemas/
│   ├── registry.schema.json
│   └── skin.schema.json
└── docs/
~~~

### skin/<id>

Each skin has an independent branch, such as skin/obsidian. The published tree contains only the skin package:

~~~text
/
├── eightfold.skin.json
├── README.md
├── theme.json
├── tokens/
│   ├── colors.json
│   └── typography.json
└── presets/
    └── default.json
~~~

The registry records the branch for discovery and a full commit SHA for reproducible installation. Branches may move; a published commit does not.

## Installation flow

Harness:

1. Fetches and validates registry.json.
2. Resolves the selected skin and pinned source commit.
3. Downloads one archive instead of cloning Armoury.
4. Validates eightfold.skin.json and referenced assets.
5. Extracts the skin into local Armoury state.
6. Records the skin ID, version, and source commit.

This keeps skins small, auditable, and independently removable.

## Publish a skin

1. Create skin/<id> as a skin-only branch.
2. Add the root eightfold.skin.json manifest.
3. Add token, theme, preset, and documentation files.
4. Validate the manifest against schemas/skin.schema.json.
5. Commit and push the branch.
6. Add the skin to registry.json on main.
7. Pin the registry entry to the full 40-character commit SHA.

See [Architecture](docs/architecture.md) and [Publishing](docs/publishing.md) for the complete format.

## Design principles

- **Visual isolation** — presentation data stays separate from behavior.
- **Small installs** — fetch one skin snapshot.
- **Reproducibility** — install exact commits.
- **Composable profiles** — select presentation independently.
- **Native integration** — Armoury does not become another plugin runtime.

## Related projects

- [Eightfold Harness](https://github.com/Eightfold-Code/eightfold-harness) — the runtime.
- [Eightfold Treasury](https://github.com/Eightfold-Code/eightfold-treasury) — the capability catalog.
