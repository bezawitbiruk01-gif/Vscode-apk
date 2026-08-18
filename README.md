# VS Code APK — standalone Android build

This repository builds a standalone Android APK from the official VS Code 1.135.0 Web distribution.

## What this is

- **Not a Chrome WebAPK.**
- **Not a launcher for vscode.dev.**
- The VS Code Web distribution is bundled into the APK under `assets/vscode-web`.
- Android launches those bundled files through `WebViewAssetLoader`.
- Runtime network requests are blocked by the app so it does not silently switch to a remote website.

## Build pipeline

GitHub Actions workflow: `.github/workflows/build-offline-apk.yml`

1. Checks out Microsoft VS Code 1.135.0.
2. Installs the pinned Node dependencies from the VS Code lockfile.
3. Builds the `vscode-web` distribution.
4. Copies the generated Web distribution into Android assets.
5. Builds `app-debug.apk` with Gradle 8.7 / Android Gradle Plugin 8.5.2.
6. Uploads the APK as the `vscode-apk-debug` workflow artifact.

## Device target

Minimum Android version: Android 8.0 (API 26). This includes Android 10 on the TECNO Spark 4.

## Important limitation

VS Code Web runs inside a browser sandbox. Bundling it into an APK can make the Web workbench and its bundled assets local, but it does **not** turn remote/browser-only services into native offline features. Marketplace, GitHub authentication, remote repositories, Copilot, and other server-backed features still need separate implementations or network access.
