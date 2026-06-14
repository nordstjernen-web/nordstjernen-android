# nordstjernen-android

Android build support for the [Nordstjernen](https://github.com/nordstjernen-web/nordstjernen)
web browser engine.

This repository hosts the CI/CD that cross-compiles **all** of Nordstjernen's
native third-party dependencies for every Android ABI and publishes them as a
downloadable **prebuilt sysroot**, so the engine's `android/scripts/build-deps.sh`
(and local developers) can consume prebuilt binaries instead of compiling the
entire dependency stack from scratch.

See [`android/README.md`](android/README.md) for the full story: which
libraries are built, how to download and consume the prebuilt sysroot, and how
to rebuild it locally.

## Quick start

```bash
# Download the prebuilt sysroot for all ABIs from the public GitHub Release
# (no auth / no `gh` needed -- just curl + tar + sha256sum):
export NORDSTJERNEN_ANDROID_SYSROOT="$HOME/.cache/nordstjernen-android-sysroot"
android/scripts/fetch-prebuilt-deps.sh --sysroot "$NORDSTJERNEN_ANDROID_SYSROOT"
```

Then point the engine's build at `$NORDSTJERNEN_ANDROID_SYSROOT` and run
`build-deps.sh` as usual. The sysroots are published as assets on the rolling
[`sysroot-latest`](https://github.com/nordstjernen-web/nordstjernen-android/releases/tag/sysroot-latest)
release by CI on every successful build of `main`.

- **Targets:** NDK `27.3.13750724` (r27); ABIs `arm64-v8a`, `armeabi-v7a`,
  `x86_64`, `x86`; minSdk 35; 16 KB page size.
- **Pinned versions + checksums:** [`android/deps/manifest.txt`](android/deps/manifest.txt).
- **CI:** [`.github/workflows/build-deps.yml`](.github/workflows/build-deps.yml).
