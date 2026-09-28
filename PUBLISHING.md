# Publishing (for the maintainer)

This repository is AuraFit's public home: the APKs (Releases), read by the app's built-in updater, and the website (GitHub Pages: this README as the home page, the privacy policy, account deletion). The app's code is in a private repository.

## A release

The in-app updater (`src/lib/updates.ts` in the code repository, which reads `expo.extra.releasesRepo`) only accepts a release that looks exactly like this:

1. **Tag and title:** `v<version>`, e.g. `v2.0.0` — the same as `expo.version` in the app's `app.json`.
2. **One asset:** `auraFit_V<version>.apk`, e.g. `auraFit_V2.0.0.apk` — the file `npm run release:android` puts in the code repository's `apk/` folder, signed with AuraFit's own key.
3. A normal release (not a draft, not a pre-release), so it's the one `releases/latest` returns.

The app checks the APK's SHA-256 (GitHub records it for every uploaded asset), its package name, its signing key and that its version code is higher before installing, so a wrong or tampered file is refused.

From the code repository, with the GitHub CLI:

```sh
gh release create v2.0.0 apk/auraFit_V2.0.0.apk --repo AmerZuher/aurafit-releases --title v2.0.0 --notes-file notes.md
```

Or on github.com: Releases → Draft a new release → tag `v2.0.0` → title `v2.0.0` → attach the APK → Publish.

The only GitHub Action here, `.github/workflows/check-release.yml`, checks each published release (tag `vX.Y.Z`, not a pre-release, an asset `auraFit_VX.Y.Z.apk`) and fails — GitHub emails you — if the app couldn't use it. Nothing is built here: the APK is built on the maintainer's machine, where the signing key stays.

Issues use the forms in `.github/ISSUE_TEMPLATE/` (a problem, an idea); account and privacy questions go to email.

## The website

Settings → Pages → Deploy from a branch, `main`, `/ (root)`. Pages:

- `/` — this repository's README
- `/privacy/`, `/privacy-ar/` — the privacy policy
- `/delete-account/` — how to delete an account

Don't edit `privacy.md` or `privacy-ar.md` by hand: they're generated from the text the app shows. Change `src/constants/privacyPolicy.ts` in the code repository (and its `PRIVACY_VERSION`), run `npm run privacy:export`, and copy `releases-repo/` here again. The README and the images in `assets/` also come from that folder.
