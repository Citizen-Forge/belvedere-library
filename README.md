# Belvedere Library

The default type catalog for [Belvedere](https://github.com/Citizen-Forge/belvedere) — a system
inventory, visualization, and monitoring tool.

This repository contains **no code**. It is data only: a tree of type definitions describing
hardware, software, and cloud-provider entities, which Belvedere (or any compatible tool) loads
directly from this repo.

Belvedere treats libraries as configurable sources — this repo is the default, but users can add
any other repo that follows the same format under their own "Libraries" settings, so vendors or
community members can publish and share their own type trees independently of this repo.

## Layout

```
types/
  hardware/
    core.hardware.yaml
    core.server.yaml
    ...
  software/
    core.software.yaml
    core.os.yaml
    ...
  cloud-provider/
    core.cloud-provider.yaml
    core.aws.yaml
    ...
```

Files are grouped by root category for readability, but the tree structure comes entirely from
each type's `extends` field, not from directory nesting.

## Type file format

One YAML file per type:

```yaml
id: core/server              # "<namespace>/<slug>", globally unique across every configured library
name: Server
root: hardware                # hardware | software | cloud-provider
extends: core/hardware        # id of the parent type; omit only for the three root types
icon: null                    # icon id/URL; if omitted, inherited from the nearest ancestor that sets one
description: A physical or virtual machine that provides compute resources.
version: 0.1.0
attributes:
  - key: cpu
    label: CPU
    dataType: string
  - key: ramGb
    label: RAM
    dataType: number
    unit: GB
```

`attributes` declared on a type are added to (not a replacement for) the attributes inherited from
its ancestors; a child redeclaring an inherited `key` overrides that one field.

`dataType` is one of: `string`, `number`, `boolean`, `enum` (with an `options` list), `reference`
(a pointer to another instance, e.g. "hosted on").

## Publishing your own types

Fork this repo (or start your own following the same `types/` layout) and add YAML files under a
namespace that's yours, extending any existing type by its `id`. Point Belvedere's Libraries
settings at your repo URL to load it alongside — or instead of — this one.

## License

[PolyForm Noncommercial 1.0.0](LICENSE).
