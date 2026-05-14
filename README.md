# Dusklight — Android

This repository is the **Android port** of [Dusklight](https://github.com/TwilitRealm/dusklight), a reverse-engineered reimplementation of Twilight Princess.

> [!IMPORTANT]
> Dusklight does *not* provide any copyrighted assets. You must provide your own copy of the original game.

> [!IMPORTANT]
> Dusklight requires a GPU with Vulkan or OpenGL ES 3.0 support. Devices with Adreno GPUs may experience issues.

---

## Supported Architectures

| ABI | Devices |
|-----|---------|
| `arm64-v8a` | All modern Android phones (64-bit ARM) |
| `armeabi-v7a` | Older Android phones (32-bit ARM) |

Minimum Android version: **API 26 (Android 8.0 Oreo)**

---

## Installation

1. Download the APK for your device from [Releases](../../releases)
   - `arm64-v8a` → modern devices (2015+)
   - `armeabi-v7a` → older 32-bit devices
2. Enable **Install from unknown sources** in your Android settings
3. Install the APK
4. Launch Dusklight
5. Press **Select Disc Image** and provide the path to your supported game dump
6. Press **Play**!

### Supported Game Versions

| Version | SHA-1 |
|---------|-------|
| GameCube USA | `75edd3ddff41f125d1b4ce1a40378f1b565519e7` |
| GameCube EUR | `2601822a488eeb86fb89db16ca8f29c2c953e1ca` |

---

## Building from Source

### Prerequisites

- Android SDK (`ANDROID_HOME`)
- Android NDK `29.0.14206865` (`ANDROID_NDK_VERSION`)
- JDK 17+
- CMake 3.21+
- Ninja
- Rust (with Android targets)

```bash
export ANDROID_HOME="$HOME/Android/Sdk"
export ANDROID_NDK_VERSION="29.0.14206865"
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk"
```

### Install Rust Targets

```bash
rustup target add aarch64-linux-android   # arm64-v8a
rustup target add armv7-linux-androideabi  # armeabi-v7a
```

### Build Native Library

This repo is a subpath of the main Dusklight source tree. Clone the full repo first:

```bash
git clone --recursive https://github.com/TwilitRealm/dusklight
cd dusklight
```

**arm64-v8a:**
```bash
cmake --preset android-arm64
cmake --build --preset android-arm64
```

**armeabi-v7a:**
```bash
cmake --preset android-armeabi-v7a
cmake --build --preset android-armeabi-v7a
```

### Stage Libraries

```bash
# arm64 only
ANDROID_STAGE_ABIS="arm64-v8a" platforms/android/scripts/stage-jni-libs.sh

# armeabi-v7a only
ANDROID_STAGE_ABIS="armeabi-v7a" platforms/android/scripts/stage-jni-libs.sh

# Both
ANDROID_STAGE_ABIS="arm64-v8a armeabi-v7a" platforms/android/scripts/stage-jni-libs.sh
```

### Build APK

```bash
cd platforms/android
./gradlew :app:assembleRelease
```

Output APKs:
- `app/build/outputs/apk/release/app-arm64-v8a-release-unsigned.apk`
- `app/build/outputs/apk/release/app-armeabi-v7a-release-unsigned.apk`

---

## Launch with Runtime Args (adb)

```bash
adb shell am start -n dev.twilitrealm.dusk/.DuskActivity \
  --es dusk_args "--backend vulkan"
```

Supported extras:
- `dusk_args` — single shell-like argument string
- `dusk_argv` — string-array argv

---

## Credits

See the [main repository](https://github.com/TwilitRealm/dusklight) for full credits.
