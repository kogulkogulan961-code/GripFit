# GripFit: Android (Capacitor)

Offline AI push-up/squat lock screen. MediaPipe Pose runs fully on-device from `www/mediapipe/`.

## Layout
```
www/index.html            the app (loads ./mediapipe/pose.js, no CDN)
scripts/copy-mediapipe.js bundles pose.js, WASM and .tflite models into www/mediapipe
scripts/patch-manifest.js adds android.permission.CAMERA to AndroidManifest.xml
scripts/setup.js          one command: bundle, add Android, patch, sync
capacitor.config.json     appId com.gripfit.app
.github/workflows/        builds the APK in the cloud
```

## Option A: Build on a computer
Install Node 18+, JDK 17 and Android Studio (for the Android SDK).
```
npm install
npm run setup
cd android
./gradlew assembleDebug        (Windows: gradlew.bat assembleDebug)
```
APK: `android/app/build/outputs/apk/debug/app-debug.apk`
Or run `npm run open`, then Build > Build APK(s) in Android Studio.

## Option B: Build from a phone (no PC), GitHub Actions
1. Create a free GitHub account, then a new repository.
2. Upload every file in this folder (use "Add file > Upload files"; keep the `.github/workflows` path).
3. Open the **Actions** tab, pick "Build GripFit APK", tap **Run workflow**.
4. After ~5 minutes open the finished run and download the **GripFit-debug-apk** artifact (a zip).
5. Unzip, open `app-debug.apk`, and allow "Install unknown apps" when asked.

## Notes
- The camera permission prompt appears the first time a locked app is opened.
- The debug APK is fine for personal use. For Play Store, create a keystore and use `./gradlew bundleRelease`.
- The manifest lines added are:
  `android.permission.CAMERA`, `android.permission.INTERNET`, and camera `uses-feature` flags.
- App locking is still simulated inside GripFit. Blocking other installed apps needs an Android
  AccessibilityService or UsageStats plugin, which is a separate native addition.
