# s3r3nity-labs Flatpak repository

**Install page:** https://s3r3nity-labs.github.io/flatpak/

A signed Flatpak repository on GitHub Pages for apps by s3r3nity-labs:

| App | ID |
| --- | --- |
| [Home Sync](https://github.com/s3r3nity-labs/linux-home-cloud-storage-sync) | `org.s3r3nity.HomeSync` |
| [MangoHud FPS Switch](https://github.com/s3r3nity-labs/mangohud-fps-switch) | `org.s3r3nity.MangoHudFPSSwitch` |

```fish
flatpak install --user https://s3r3nity-labs.github.io/flatpak/org.s3r3nity.HomeSync.flatpakref
flatpak install --user https://s3r3nity-labs.github.io/flatpak/org.s3r3nity.MangoHudFPSSwitch.flatpakref
```

## How it works

- `repo/` is the Flatpak (OSTree) repository. Don't edit it by hand: each app's release
  workflow builds into it, signs it, prunes old builds and pushes it here
  (`.github/workflows/flatpak.yml` in each app's repository). Runtimes come from Flathub
  (`RuntimeRepo` in the `.flatpakref`).
- `s3r3nity-labs.flatpakrepo` adds the repository; each app's `<id>.flatpakref` installs
  it (and adds the repository). All carry the public key.
- Signing key: RSA 4096, fingerprint `DA8B 9C3E E876 F9AF B53D 4D4A 0424 DDA6 99CD 08FE`.
  Never replace it: installed apps only accept updates signed with it. The private key is
  the organization secret `FLATPAK_GPG_PRIVATE_KEY`; pushes use the organization secret
  `FLATPAK_REPO_TOKEN` (fine-grained, Contents read/write on this repository only). Both
  are shared with the app repositories that publish here.
- Several apps push here: MangoHud FPS Switch's workflow builds into its own repository
  first and retries on a rejected push.
- `.nojekyll`: GitHub Pages serves the files as they are.

To add an app: publish it from its own repository's workflow (see MangoHud FPS Switch's),
give that repository access to both organization secrets, add a `.flatpakref` and a card in
`index.html`.
