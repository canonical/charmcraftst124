# charmcraftst124

> [!CAUTION]
> This project is **deprecated**. Use [charmcraft](https://github.com/canonical/charmcraft) >=3.3.0 instead

Pack multi-base charms (that include base Ubuntu 24.04) with a single charmcraft.yaml and git branch using the upcoming [ST124 - Multi-base platforms in craft tools](https://docs.google.com/document/d/1QVHxZumruKVZ3yJ2C74qWhvs-ye5I9S6avMBDHs2YcQ/edit) "shorthand notation" charmcraft.yaml syntax

## Installation
Install `pipx`: https://pipx.pypa.io/stable/installation/
```
pipx install charmcraftst124
```

## Usage
### Step 1: Update charmcraft.yaml to supported syntax
Only ST124 "shorthand notation" syntax is supported

#### Example
```yaml
platforms:
  ubuntu@22.04:amd64:
  ubuntu@22.04:arm64:
  ubuntu@24.04:amd64:
  ubuntu@24.04:arm64:
```

Under the `platforms` key, `build-on` and `build-for` syntax are not supported

The `base` and `bases` keys are not supported

### Step 2: Pack platform
```
charmcraftst124 pack --platform ubuntu@22.04:amd64
```

See `charmcraftst124 pack --help` for more information

To pack multiple platforms, run `charmcraftst124 pack` once for each platform

## Why is only "shorthand notation" syntax supported?
"Shorthand notation" is used when `build-on` is identical to `build-for`. (For example, "ubuntu@22.04:amd64" means build on Ubuntu 22.04 amd64 and build for Ubuntu 22.04 amd64.)

For charms that depend (directly or indirectly) on Python packages with C extensions (e.g. pyyaml), charmcraft will build wheels where the C extensions only work on `build-on`.

For example, if a charm is built with
```yaml
platforms:
  foo:
    build-on: ubuntu@22.04:amd64
    build-for:
      - ubuntu@22.04:amd64
      - ubuntu@22.04:arm64
```
the wheels in the *.charm file will contain C extensions that only work on amd64, not arm64.

The vast majority of charms have at least one Python dependency with C extensions, so the vast majority of charms should use "shorthand notation".
