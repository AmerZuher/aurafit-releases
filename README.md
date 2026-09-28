# AuraFit releases

Public home of **AuraFit**, a food diary and fitness tracker for Android (English and Arabic). The app's code is in a private repository; this one holds what must stay public:

- **[Releases](https://github.com/AmerZuher/aurafit-releases/releases)** — the Android APKs. The app's built-in updater reads the latest release here (`app.json` → `expo.extra.releasesRepo` in the code repository).
- **The website** (GitHub Pages): the [privacy policy](https://amerzuher.github.io/aurafit-releases/privacy/) ([العربية](https://amerzuher.github.io/aurafit-releases/privacy-ar/)) and [how to delete your account](https://amerzuher.github.io/aurafit-releases/delete-account/) — the links Google Play and Google sign-in ask for.

## Publishing a release

The in-app updater only accepts a release that looks exactly like this:

1. **Tag and title:** `v<version>`, e.g. `v2.0.0` — the same as `expo.version` in the app's `app.json`.
2. **One asset:** `auraFit_V<version>.apk`, e.g. `auraFit_V2.0.0.apk` — the file `npm run release:android` puts in the code repository's `apk/` folder, signed with AuraFit's own key.
3. A normal release (not a draft, not a pre-release), so it's the one `releases/latest` returns.

The app checks the APK's SHA-256 (GitHub records it for every uploaded asset), its package name, its signing key and that its version code is higher before installing, so a wrong or tampered file is refused.

With the GitHub CLI, from the code repository:

```sh
gh release create v2.0.0 apk/auraFit_V2.0.0.apk --repo AmerZuher/aurafit-releases --title v2.0.0 --notes "What's new…"
```

## Updating the privacy policy

Don't edit `privacy.md` or `privacy-ar.md` here by hand. They're generated from the text the app shows: change `src/constants/privacyPolicy.ts` in the code repository (and its `PRIVACY_VERSION`), run `npm run privacy:export`, and copy `releases-repo/privacy.md` and `releases-repo/privacy-ar.md` here.
