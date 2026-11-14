# s3r3nity-labs Flatpak repository

**Install page:** https://s3r3nity-labs.github.io/flatpak/

A signed Flatpak repository on GitHub Pages for apps by s3r3nity-labs:

| App | ID |
| --- | --- |
| [Home Sync](https://github.com/s3r3nity-labs/linux-home-cloud-storage-sync) | `org.s3r3nity.HomeSync` |

```fish
flatpak install --user https://s3r3nity-labs.github.io/flatpak/org.s3r3nity.HomeSync.flatpakref
```

## How it works

- `repo/` is the Flatpak (OSTree) repository. Don't edit it by hand: each app's release
  workflow builds into it, signs it, prunes old builds and pushes it here (for Home Sync:
  `.github/workflows/flatpak.yml` in linux-home-cloud-storage-sync). Runtimes come from
  Flathub (`RuntimeRepo` in the `.flatpakref`).
- `s3r3nity-labs.flatpakrepo` adds the repository, `org.s3r3nity.HomeSync.flatpakref`
  installs Home Sync (and adds the repository). Both carry the public key.
- Signing key: RSA 4096, fingerprint `DA8B 9C3E E876 F9AF B53D 4D4A 0424 DDA6 99CD 08FE`.
  The private key is the secret `FLATPAK_GPG_PRIVATE_KEY` in each app repository that
  publishes here; pushes use the fine-grained token `FLATPAK_REPO_TOKEN` (Contents
  read/write on this repository only).
- `.nojekyll`: GitHub Pages serves the files as they are.

To add an app: build it in its own repository's workflow into `repo/` with the same key
(see Home Sync's), add a `.flatpakref` and a card in `index.html`.
