# UC2000Keyboard

Minimal steps to build the debug APK on a fresh Debian install.

## 1) Install system packages

```bash
sudo apt update
sudo apt install -y git wget unzip openjdk-21-jdk
```

## 2) Install Android SDK command-line tools

```bash
export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
mkdir -p "$ANDROID_SDK_ROOT/cmdline-tools"

wget -O /tmp/cmdline-tools.zip \
  https://dl.google.com/android/repository/commandlinetools-linux-13114758_latest.zip

unzip -q /tmp/cmdline-tools.zip -d "$ANDROID_SDK_ROOT/cmdline-tools"
mv "$ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools" "$ANDROID_SDK_ROOT/cmdline-tools/latest"

export PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH"
```

## 3) Install required Android SDK components

```bash
yes | sdkmanager --licenses >/dev/null
sdkmanager "platform-tools" "platforms;android-36" "build-tools;36.0.0"
```

## 4) Build

```bash
printf "sdk.dir=%s\n" "$ANDROID_SDK_ROOT" > local.properties
chmod +x ./gradlew
./gradlew assembleDebug
```

APK output:

- app/build/outputs/apk/debug/app-debug.apk
