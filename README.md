# BlueMap Folia Regions

A Paper plugin that visualizes [Folia](https://github.com/PaperMC/Folia)'s threaded tick regions on [BlueMap](https://bluemap.bluecolored.de/)'s web map in real time.

<img width="862" alt="folia_regions" src="https://github.com/user-attachments/assets/0e6ae120-f303-40a2-b47a-77a4e8627c4f" />

> **Fork note:** This is a maintained fork of [PfauMC/bluemap-folia-regions](https://github.com/PfauMC/bluemap-folia-regions) with **Folia 26.2** support. Upstream still targets older Canvas / 1.21.x tooling and crashes on 26.2 with `IllegalAccessError` when reading private `ChunkPos` fields.

## Overview

Folia splits the Minecraft world into independent regions that tick in parallel across multiple threads. This plugin renders those region boundaries as polygon overlays on BlueMap, giving server administrators a live view of how their world is partitioned.

Each region marker displays:
- Number of sections and chunks
- Entity and player count
- TPS (Ticks Per Second)
- MSPT (Milliseconds Per Tick)

Markers refresh every 5 seconds automatically.

## Requirements

- [Folia](https://github.com/PaperMC/Folia) **26.2** (tested against `26.2.build.7-beta`)
- [BlueMap](https://bluemap.bluecolored.de/) plugin installed
- Java **25** (required by Folia / Paper 26.x)

## Installation

1. Build the plugin (see below) or use a release jar from this fork.
2. Place the `.jar` file into your server's `plugins` directory.
3. Remove any older `BlueMap-Folia-Regions-1.1.0.jar` (or earlier) so Folia only loads **1.2.0+**.
4. Restart the server.

The "Folia Regions" marker set will appear on your BlueMap (hidden by default — toggle it on from the map sidebar).

## What changed in this fork (v1.2.0)

| Area | Change |
|------|--------|
| Build target | Switched from Canvas `1.21.11` to Folia `paperweight.foliaDevBundle("26.2.build.7-beta")` |
| Plugin API | `api-version` updated from `1.20` → `26.2` |
| Tooling | `paperweight-userdev` bumped to `2.0.0-beta.23`; Java toolchain set to 25 |
| Reobfuscation | Removed `reobfJar` — Minecraft 26.1+ ships unobfuscated, so remapping is no longer supported |
| `ChunkPos` crash | Replaced private field access (`centerChunk.x` / `.z`) with record accessors `x()` / `z()`, and `ChunkPos.asLong` with `ChunkPos.pack` |
| Tick metrics | Updated `TickData` import to `ca.spottedleaf.common.time.TickData` (package moved in leafpile / Folia 26.2) |

### Fixed runtime error

On Folia 26.2, the upstream jar failed every 5 seconds with:

```text
java.lang.IllegalAccessError: class ...BlueMapFoliaRegionsPlugin tried to access private field net.minecraft.world.level.ChunkPos.x
```

`ChunkPos` is now a Java record with private components. Direct field reads no longer work from a plugin classloader. This fork uses the public accessors instead, so region markers update normally on 26.2.

## Building

```bash
./gradlew build
```

The compiled jar will be in `build/libs/` (for example `BlueMap-Folia-Regions-1.2.0.jar`).

## Upstream

- Original project: [PfauMC/bluemap-folia-regions](https://github.com/PfauMC/bluemap-folia-regions)
- License remains MIT

## License

[MIT](LICENSE)
