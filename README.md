# Syncthing Wrapped

> Vibe coded experiment with syncthing given the current syncthing Android fork situation 

An Android wrapper app for Syncthing that runs the original Syncthing binary in a WebView with proper background service management, battery optimization handling, and CI/CD pipeline.

## Features

- **Native Syncthing**: Bundles and runs the official Syncthing binary
- **WebView Interface**: Displays Syncthing's web UI in a native Android WebView
- **Background Service**: Keeps Syncthing running as a foreground service to prevent OS from killing it
- **Battery Optimization**: Prompts user to disable battery optimization for uninterrupted operation
- **Wake Lock**: Maintains CPU wake lock to ensure continuous syncing
- **Multi-Architecture Support**: Includes binaries for ARM64, ARMv7, x86_64, and x86
- **CI/CD Pipeline**: Automated build and testing with GitHub Actions

## Architecture

The app consists of:
- **MainActivity**: Hosts the WebView that displays Syncthing's web interface (http://127.0.0.1:8384)
- **SyncthingService**: Foreground service that extracts and runs the Syncthing binary
- **Syncthing Binaries**: Native binaries bundled in assets for different Android architectures

## Build Instructions

### Prerequisites

- JDK 17 or higher
- Android SDK (API level 34)
- Gradle 8.2+

### Building the APK

1. **Clone the repository**:
   ```bash
   git clone https://github.com/vs4vijay/syncthing-wrapped.git
   cd syncthing-wrapped
   ```

2. **Download Syncthing binaries**:
   ```bash
   ./scripts/download-syncthing.sh
   ```
   This will download the latest Syncthing binaries for all supported architectures.

3. **Build debug APK**:
   ```bash
   ./gradlew assembleDebug
   ```
   The APK will be located at: `app/build/outputs/apk/debug/app-debug.apk`

4. **Build release APK**:
   ```bash
   ./gradlew assembleRelease
   ```
   The APK will be located at: `app/build/outputs/apk/release/app-release-unsigned.apk`

### Installing on Device

```bash
adb install app/build/outputs/apk/debug/app-debug.apk
```

## Development

### Project Structure

```
syncthing-wrapped/
├── app/
│   ├── src/main/
│   │   ├── java/com/syncthing/wrapped/
│   │   │   ├── MainActivity.java       # Main activity with WebView
│   │   │   └── SyncthingService.java   # Background service
│   │   ├── res/                         # Android resources
│   │   ├── assets/                      # Syncthing binaries
│   │   └── AndroidManifest.xml
│   └── build.gradle
├── scripts/
│   └── download-syncthing.sh           # Script to download Syncthing binaries
├── .github/workflows/
│   └── android-ci.yml                  # CI/CD pipeline
├── build.gradle
├── settings.gradle
└── README.md
```

### Running Lint

```bash
./gradlew lint
```

### Running Tests

```bash
./gradlew test
```

## CI/CD Pipeline

The project includes a GitHub Actions workflow that:
1. **Lint Check**: Runs Android lint on every PR and push
2. **Build**: Compiles debug and release APKs
3. **Test**: Runs unit tests
4. **Artifacts**: Uploads built APKs as artifacts

### Workflow Triggers
- Push to `main` or `develop` branches
- Pull requests to `main` or `develop` branches

### Downloading Build Artifacts

After a successful CI build, you can download the APK from the GitHub Actions artifacts:
1. Go to the Actions tab in the repository
2. Click on the latest workflow run
3. Download the `app-debug` or `app-release` artifact

## Permissions

The app requires the following permissions:
- `INTERNET`: To allow Syncthing to sync with other devices
- `ACCESS_NETWORK_STATE`: To check network connectivity
- `WAKE_LOCK`: To keep CPU running for continuous sync
- `FOREGROUND_SERVICE`: To run as a foreground service
- `FOREGROUND_SERVICE_DATA_SYNC`: To indicate the service type
- `POST_NOTIFICATIONS`: To show service notification (Android 13+)
- `READ/WRITE_EXTERNAL_STORAGE`: For file synchronization
- `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`: To request battery optimization exemption

## Usage

1. Install the APK on your Android device
2. Launch "Syncthing Wrapped"
3. Grant necessary permissions
4. The app will prompt you to disable battery optimization - this is recommended
5. The Syncthing web UI will load (may take a few seconds on first launch)
6. Configure Syncthing through the web interface
7. The app will keep Syncthing running in the background

## Battery Optimization

For optimal performance, it's recommended to disable battery optimization for this app:
- The app will automatically prompt you on first launch
- You can also manually disable it in: Settings → Apps → Syncthing Wrapped → Battery → Unrestricted

## Troubleshooting

### Syncthing won't start
- Check if the app has all required permissions
- Ensure battery optimization is disabled
- Check logcat for errors: `adb logcat | grep Syncthing`

### WebView shows error
- Wait a few seconds for Syncthing to initialize
- Pull to refresh or restart the app
- Check if port 8384 is not blocked

### App gets killed in background
- Disable battery optimization for the app
- Check manufacturer-specific battery settings (e.g., Xiaomi's autostart, Huawei's protected apps)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the terms specified in the LICENSE file.

## Acknowledgments

- [Syncthing](https://syncthing.net/): The core synchronization engine
- [Syncthing-Android](https://github.com/syncthing/syncthing-android): Inspiration for the wrapper approach

## Version Information

- **App Version**: 1.0.0
- **Syncthing Version**: v1.27.2
- **Target Android SDK**: 34 (Android 14)
- **Minimum Android SDK**: 24 (Android 7.0)