# VPM Workflows

This repository is a demonstration of workflows for [VPM (VRChat Package Manager)](https://vcc.docs.vrchat.com/vpm).

## Use package manifest

[Action](./.github/actions/use-package-manifest/action.yml)

Parses package manifest (`package.json`) and extract package name, id, version, etc.

```yaml
- uses: Tsukina-7mochi/vpm-workflow/.github/actions/use-package-manifest
  id: manifest
  with:
    filename: ./Assets/MyPAckage/package.json

- run: echo "version: ${{ steps.manifest.outputs.version }}"
```

## Validating package manifest

[Action](./.github/actions/validate-package-manifest/action.yml)

Checks if the package manifest (`package.json`) has the [specific fields](https://vcc.docs.vrchat.com/vpm/packages#package-format).
Also checks the version of the manifest, if provided, and the package URL with a pattern.

### URL check

Specify pattern as `url-pattern` to check if the url string matches the pattern.
You can use `name`, `displayName`, `version` as placeholders.

```yaml
- uses: Tsukina-7mochi/vpm-workflow/.github/actions/validate-package-manifest
  with:
    version: ${{ github.ref_name }}
    filename: Assets/MyPacakge/package.json
    url-pattern: "https://example.com/artifact/{name}.v{version}.zip"
```

## Validating repository manifest

[Action](./.github/actions/validate-repository-manifest/action.yml)

Checks that repository manifest (`vpm.json` in this repository) has the [specific fields](https://vcc.docs.vrchat.com/vpm/packages#package-format).
This action assumes that multiple packages are managed in a monorepo and that the repository manifest is served statically.

```yaml
- uses: Tsukina-7mochi/vpm-workflow/.github/actions/validate-repository-manifest
  with:
    filename: "vpm.json"
```

## Checking repository includes a package

[Action](./.github/actions/check-repository-includes-package/action.yml)

Checks if a package manifest is included in the repository manifest as the `packages.{package name}.{version}`.

```yaml
- uses: Tsukina-7mochi/vpm-workflow/.github/actions/check-repository-includes-package
  with:
    repository-filename: "vpm.json"
    package-filename: "Assets/MyPackage/pacakge.json"
```
