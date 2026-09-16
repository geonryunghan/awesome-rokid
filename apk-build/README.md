# Rokid APK Build Lab

This branch adds a reproducible GitHub Actions build for a small set of Android projects referenced by `awesome-rokid`. It does not vendor or modify upstream source code. Each build clones an exact upstream commit, builds APKs, records SHA-256 checksums, and uploads the results as short-lived GitHub Actions artifacts.

## Selected projects

| Project | Upstream commit | Build target | Expected use |
| --- | --- | --- | --- |
| Rokid-GMaps | `Anezium/Rokid-GMaps@f8598518670133d38bec685fb6708af5553fb529` | `assembleDebug` | Android phone companion + Rokid glasses HUD |
| Rokid-Maps | `chartmann1590/Rokid-Maps@3542db3b47a5995739025a0c1874df3d59f7f1c9` | `assembleDebug` | Android phone companion + Rokid glasses HUD |
| AssistBridge | `Anezium/AssistBridge@7b180625e899a86293184b8535131f95ca7e9d2b` | `:phone-app:assembleRelease :glasses-app:assembleRelease` | Android phone accessibility relay + Rokid glasses HUD |

## Artifacts

A successful workflow run creates three artifacts, retained for 14 days:

- `apk-builds-rokid-gmaps`
- `apk-builds-rokid-maps`
- `apk-builds-assistbridge`

Each artifact contains all APKs found under the upstream Gradle `build/outputs/apk` directories plus `SHA256SUMS.txt`.

## Installation outline

### Phone

Enable installation from the browser/file-manager source you use, then install the APK whose filename contains `phone` or `phone-app`. ADB is preferable for repeatable testing:

```bash
adb install -r <phone-apk>
```

### Rokid glasses

Connect the glasses to a development host with ADB enabled and identify the serial:

```bash
adb devices
adb -s <glasses-serial> install -r <glasses-apk>
```

`Rokid-Maps` also documents a phone-side APK transfer/update path over Bluetooth. `AssistBridge` bundles the glasses APK into the phone app for its CXR-L install/update path when authorized.

## Important notes

- These are personal sideload/test builds. Review each upstream project's license before redistributing APKs.
- No Rokid or Google credentials are injected by this workflow.
- Rokid-Maps can operate without Rokid SDK credentials according to its upstream documentation.
- Rokid-GMaps can use its OSM/OSRM fallback without a Google API key; Google Places/Routes are optional and configured on the phone.
- AssistBridge requires the Hi Rokid/CXR-L authorization flow and Android Accessibility permissions for its intended operation.
- If GitHub Actions is disabled on this fork, enable Actions for the repository and rerun the workflow manually.

## Provenance

The workflow intentionally pins exact upstream SHAs. Updating a project should be done as an explicit commit changing the SHA, followed by a fresh build and installation test rather than following an unpinned branch head.
