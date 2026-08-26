# Armoury architecture

Armoury gives Eightfold Harness a stable place to discover and install visual skins without mixing presentation concerns into the runtime or capability catalog.

## The three repositories

| Repository | Responsibility |
| --- | --- |
| [eightfold-harness](https://github.com/Eightfold-Code/eightfold-harness) | Runtime, orchestration, profiles, and lifecycle |
| [eightfold-treasury](https://github.com/Eightfold-Code/eightfold-treasury) | Downloadable adaptations and capability bundles |
| [eightfold-armoury](https://github.com/Eightfold-Code/eightfold-armoury) | Downloadable skins, themes, and visual presets |

The rule is simple: Treasury changes what Harness can do; Armoury changes how Harness presents itself.

## Branch model

The `main` branch is a small catalog surface. It contains `registry.json`, schemas, and documentation. Each skin is published from a dedicated `skin/<id>` branch.

The published branch tip should contain only the skin snapshot:

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

The registry pins the full commit SHA of the snapshot. Harness may use the branch for discovery, but installation must resolve the pinned commit for reproducibility.

## Skin package boundaries

A skin may contain:

- design tokens for color, typography, spacing, radii, elevation, and motion;
- component or layout presets;
- icon and visual asset references;
- metadata required to render the theme.

A skin must not contain executable plugins, network calls, secrets, authentication logic, permission changes, or capability behavior. Capability work belongs in Treasury.

## Installation flow

1. Harness fetches and validates Armoury's `registry.json`.
2. It resolves a skin ID or collection.
3. It verifies the registry's pinned source commit.
4. It downloads the archive for that commit.
5. It validates `eightfold.skin.json` and referenced paths.
6. It stores the snapshot under `.eightfold/skins/<id>`.
7. It activates the selected skin in the profile without changing installed adaptations.

Because the skin is an independent package, changing or removing a skin does not change the behavior of the Harness runtime or its Treasury adaptations.

## Profile composition

Visual selection remains a separate profile concern:

~~~json
{
  "adaptations": ["session-search", "developer-tools"],
  "skin": "obsidian"
}
~~~

This allows a user to update a visual layer without reinstalling capabilities, and allows a capability bundle to work with any compatible skin.
