# Meson cross-files

This directory holds the per-ABI Meson cross-files used to cross-compile the
dependency stack with the Android NDK r27 clang toolchain.

The files `<abi>.cross` (e.g. `arm64-v8a.cross`) are **generated** by
[`../scripts/gen-cross-files.sh`](../scripts/gen-cross-files.sh) because they
embed absolute, machine-specific paths (the NDK toolchain location and the
install-sysroot `pkg_config_libdir`). They are therefore git-ignored.

To (re)generate them:

```bash
export ANDROID_NDK_HOME=/path/to/ndk/27.3.13750724
export NORDSTJERNEN_ANDROID_SYSROOT="$PWD/sysroot"
android/scripts/gen-cross-files.sh            # all four ABIs
android/scripts/gen-cross-files.sh arm64-v8a  # a single ABI
```

`build-android-deps.sh` regenerates the relevant cross-file automatically
before building, so you normally don't need to run this by hand.

Each generated cross-file pins:

- `[binaries]` — the NDK clang driver (`<triple><API>-clang(++)`), `llvm-ar`,
  `llvm-strip`, `llvm-ranlib`, `llvm-nm`, `ld.lld`, and `pkg-config`.
- `[built-in options]` — `-fPIC`, the Android API macro, and the mandatory
  `-Wl,-z,max-page-size=16384` (16 KB pages) on every link.
- `[properties]` — `pkg_config_libdir` pointing at the per-ABI install prefix.
- `[host_machine]` — the correct `cpu_family` / `cpu` / `endian` for the ABI.
