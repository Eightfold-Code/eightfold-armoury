# Publishing a skin

This is the release procedure for a skin package.

## 1. Create the package branch

Create an orphan branch so the skin snapshot is independent from the catalog history:

~~~bash
git clone https://github.com/Eightfold-Code/eightfold-armoury.git
cd eightfold-armoury
git switch --orphan skin/<id>
git rm -rf .
~~~

The branch should contain `eightfold.skin.json`, `theme.json`, a `README.md`, and only the token, preset, and asset files referenced by the manifest.

## 2. Validate the package

Before pushing, verify:

- the ID is lowercase kebab case and matches the branch name;
- the version is semantic versioning;
- every manifest path exists and stays inside the archive;
- `presentationOnly` is `true`;
- no executable code, secrets, network clients, or permissions are present;
- the package remains usable without files from `main`.

## 3. Publish the snapshot

~~~bash
git add .
git commit -m "feat(skin): publish <id> v<version>"
git push -u origin skin/<id>
git rev-parse HEAD
~~~

Keep the resulting full commit SHA. It is the immutable source reference for the registry.

## 4. Update `registry.json`

Add an entry under `skins` on `main`:

~~~json
{
  "id": "<id>",
  "name": "<display name>",
  "description": "<short description>",
  "version": "<version>",
  "source": {
    "repository": "Eightfold-Code/eightfold-armoury",
    "branch": "skin/<id>",
    "commit": "<full 40-character SHA>"
  },
  "manifest": "eightfold.skin.json",
  "compatibility": {
    "eightfoldHarness": ">=0.1.0"
  }
}
~~~

Validate the registry, review the diff, and merge the catalog update only after the skin snapshot is available at its pinned commit.

## Versioning

- Patch releases fix token values or metadata without changing the package contract.
- Minor releases add optional tokens or presets while preserving compatibility.
- Major releases change the manifest or required rendering contract.

Never move a published registry entry to a different commit without changing its version and recording the new source explicitly.
