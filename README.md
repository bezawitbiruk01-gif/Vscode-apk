# VS Code APK — standalone Android build

This repository builds a standalone Android APK from the official VS Code 1.135.0 Web distribution and preserves the complete VS Code source tree inside the APK.

## What this is

- **Not a Chrome WebAPK.**
- **Not a launcher for vscode.dev.**
- The compiled VS Code Web distribution is bundled into the APK under `assets/vscode`.
- Android launches those bundled files through `WebViewAssetLoader`.
- Runtime network requests are blocked by the app so it cannot silently switch to a remote website.
- The complete checked-out VS Code source tree is additionally preserved as `assets/vscode-main-source.zip`, together with a SHA-256 checksum and the archived file count.

## Build pipeline

GitHub Actions workflow: `.github/workflows/build-offline-apk.yml`

1. Checks out Microsoft VS Code 1.135.0.
2. Installs the pinned Node dependencies from the VS Code lockfile.
3. Runs the upstream `vscode-web` build pipeline.
4. Verifies the compiled Web workbench and its required files.
5. Creates `vscode-main-source.zip` from the complete checkout (excluding only `.git` metadata created by Git itself), records SHA-256 and file count, and embeds that archive into the APK.
6. Copies the compiled Web distribution into Android assets.
7. Builds `app-debug.apk` with Gradle 8.7 / Android Gradle Plugin 8.5.2.
8. Verifies that the APK exists and uploads it as the `vscode-apk-debug` workflow artifact.

## Device target

Minimum Android version: Android 8.0 (API 26). This includes Android 10 on the TECNO Spark 4.

## Desktop-to-Android adaptation

The repository does **not** blindly rewrite thousands of desktop source files. VS Code already has a dedicated Web build path that removes Electron/Node desktop-only pieces from the runnable Web bundle while retaining the original source. Android-specific runtime behavior is supplied by the native wrapper and local asset loader.

The APK therefore has two layers:

1. **Runnable Android/Web layer** — the compiled VS Code Web workbench and bundled Web extensions.
2. **Complete source-retention layer** — the full VS Code source archive for inspection, future Android-specific adaptation, and reproducibility.

## Important limitation

VS Code Web runs inside a browser sandbox. Bundling it into an APK can make the Web workbench and its bundled assets local, but it does **not** turn remote/browser-only services into native offline features. Marketplace, GitHub authentication, remote repositories, Copilot, and other server-backed features require separate implementations or network access.
