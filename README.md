# VS Code APK

Builds a standalone Android APK containing the VS Code Web workbench as local assets.

## Goal

- Build VS Code Web from the official Microsoft repository in GitHub Actions.
- Package the generated `vscode-web` directory into an Android WebView application.
- Produce a debug APK as a downloadable GitHub Actions artifact.
- Target Android 10+ (TECNO Spark 4 compatible baseline).

## Important

This is an offline packaging project, not a promise that every VS Code Web feature is offline. Features that inherently require network services (for example Marketplace, remote repositories, sign-in, or cloud services) remain network-dependent.

## Build

Push to GitHub and run **Actions → Build Offline VS Code APK**. The workflow builds the Web distribution and then packages it into `app-debug.apk`.
