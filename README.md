# FAHHANG 888 — auto-update mirror

Public mirror for the [FAHHANG 888](https://fahhang888.com) Tauri
auto-updater. **This repo holds only the update manifest and signed
installer assets.** The application source code lives in the private
`fahhang888` repo and is not mirrored here.

## What's here

- `dist-manifest/stable.json` — current stable channel manifest, read
  by the Tauri updater plugin once per hour by every running install
- Per-version GitHub Releases (e.g. `v3.9.1`) carrying the
  `.exe` + `.sig` as release assets — fetched on update accept

## Endpoints used by the desktop app

| Purpose | URL |
|---|---|
| Manifest | `https://raw.githubusercontent.com/nattawapong1150d/fahhang888-updates/main/dist-manifest/stable.json` |
| Installer (per release) | `https://github.com/nattawapong1150d/fahhang888-updates/releases/download/v<version>/FAHHANG.888_<version>_x64-setup.exe` |

Both are baked into the Tauri config of v3.9.1+ desktop builds.
**v3.8.x / v3.9.0 builds were tied to the old source-repo URL** and
require a manual installer upgrade once · see the source repo's
[`docs/UPDATER.md`](https://github.com/nattawapong1150d/fahhang888/blob/main/docs/UPDATER.md)
"Stranded installs" section.

## Why a separate mirror repo

The source repo is private. Tauri's updater fetches via anonymous HTTP,
so the manifest + assets need a public host. Splitting them off lets
the source stay private without losing auto-update. Decision rationale:
[ADR-0005](https://github.com/nattawapong1150d/fahhang888/blob/main/docs/adr/0005-update-distribution.md)
in the source repo.

## Release flow

See [`docs/OPERATOR_RUNBOOK.md`](https://github.com/nattawapong1150d/fahhang888/blob/main/docs/OPERATOR_RUNBOOK.md)
§3 in the source repo for the full per-release runbook.

## Verifying integrity

The .exe is signed with Tauri minisign · public key embedded in the
desktop binary. A tampered installer won't pass the client-side
verification regardless of where it's served. Don't replace
`dist-manifest/stable.json` with a hand-edited `signature` value —
generate it via `scripts/gen-update-manifest.mjs` in the source repo.
