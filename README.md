# conductor-releases

Public download host for **Conductor Studio** builds and their auto-update
manifests. This repository holds **no source code** — Studio's source is
private. The open-source engine lives at
[monstrous-media/conductor](https://github.com/monstrous-media/conductor).

## What is here

- **Releases** (`v<version>`): the signed, notarized Studio `.dmg`, the
  updater tarball (`.app.tar.gz`) and `.sha256` checksums. Uploaded by the
  private repo's release workflow; never by hand.
- **`updates/<channel>/latest.json`**: the Tauri updater manifest per channel
  (`stable`, `beta`). Served to installed copies at
  `https://getconductor.app/updates/<channel>/latest.json` through a proxy
  rewrite on the product site. The manifest's download URLs point at the
  release assets here. Every manifest is signed (minisign, key ID
  `8DCFA3FCBA9F55B5`); an installed copy rejects anything else.

## Rules

- Download URLs inside a manifest are **versioned** (`/releases/download/v<version>/...`);
  nothing at a fixed URL is ever overwritten.
- To pull a release: revert `latest.json` **and** delete the tarball asset in
  the same change, so a cached manifest resolves to a 404 instead of a stale
  download.
- Issues about Studio go to [support](https://getconductor.app/support), not here.

A manifest with `"version": "0.0.0"` and no platforms is the sentinel for
"no release published on this channel yet": the updater reads it as *no update*.
