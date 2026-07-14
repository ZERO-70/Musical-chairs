# Musical Chairs

An Expo + React Native mobile game that automates parts of a real-world **musical chairs** session using the device camera, audio playback, and on-device YOLOv8 inference.

The app can detect player count before a round, run the game loop (music start/stop + countdowns), and attempt winner detection at game end by identifying the player most likely seated on a chair.

## Key Features

- **Game flow orchestration**: intro video, game countdown, random music stops, settle timers, and automatic restart countdown.
- **Camera-powered setup**: detects people and proposes chair count (`players - 1`) before starting.
- **On-device AI inference**: uses bundled `yolov8n.onnx` with `onnxruntime-react-native`.
- **Winner detection flow**: detects persons + chairs, computes overlap, and highlights a likely winner.
- **Interactive controls**: switch camera, change songs, manual chair selection fallback.
- **Audio/visual feedback**: warning sounds, overlays, animations (Lottie), and confidence labels.

## Tech Stack

| Area | Technology |
|---|---|
| Framework | Expo SDK 52, React Native 0.76, React 18 |
| Routing | Expo Router |
| Language | TypeScript + TSX (strict mode) |
| Camera/Media | `expo-camera`, `expo-av`, `expo-video` |
| ML Inference | `onnxruntime-react-native` + YOLOv8 ONNX model |
| Image Processing | `expo-image-manipulator`, `jpeg-js`, `base-64` |
| Build/Distribution | EAS (`eas.json` profiles) |
| Testing | Jest + `jest-expo` |
| Linting | Expo lint (ESLint) |

### Language Composition (repository source, excluding `node_modules`)

- TSX: 17 files
- TypeScript: 6 files
- JavaScript: 4 files
- Kotlin/Gradle present for native Android project files

## Prerequisites

- **Node.js** (LTS recommended)
- **npm** (project ships with `package-lock.json`)
- **Expo-compatible mobile environment**:
  - Android Studio emulator and/or physical Android device
  - Xcode simulator/device for iOS (macOS only)
- Optional: **EAS CLI** for cloud/dev client builds

## Installation

```bash
npm install
```

## Running the Project

### Development

Start the Expo dev server:

```bash
npm run start
```

Run on Android:

```bash
npm run android
```

Run on iOS:

```bash
npm run ios
```

Run in web mode:

```bash
npm run web
```

### Production / Distribution (EAS)

Build profiles are defined in `eas.json`:

- `development` (internal, dev client)
- `preview` (internal)
- `production`
- `production-apk` (Android APK)

Typical usage (requires EAS CLI login/config):

```bash
eas build --platform android --profile production-apk
eas build --platform ios --profile production
```

## Configuration

### App configuration (`app.json`)

Key values currently configured:

- App name/slug/version
- Deep link scheme: `myapp`
- Android package: `com.anonymous.MusicalChairs`
- Expo Router plugin + splash screen plugin configuration
- EAS project ID in `expo.extra.eas.projectId`

### Metro configuration (`metro.config.js`)

- Adds `.onnx` to Metro asset extensions so the model can be bundled.

### Environment variables

- No required runtime `.env` variables were found for core functionality.
- `.env*.local` is gitignored for local-only overrides if needed.
- `process.env.EXPO_OS` is used in one component (`components/HapticTab.tsx`) for platform checks.

## Quality Commands

Lint:

```bash
npm run lint
```

Tests:

```bash
npm test
```

Run tests once (CI-style):

```bash
npm test -- --watchAll=false
```

> Note: The current repository contains pre-existing lint issues unrelated to this README update.

## Project Structure

```text
.
├── app/
│   ├── _layout.tsx                 # Root navigation/theme/splash handling
│   └── (tabs)/
│       ├── _layout.tsx             # Stack setup for index + CameraScreen
│       ├── index.tsx               # Welcome/landing screen
│       └── CameraScreen.tsx        # Main game + camera + detection workflow
├── assets/
│   ├── models/yolov8n.onnx         # Bundled YOLOv8 model
│   ├── *.mp3                       # Game/warning music tracks
│   ├── intro.mp4                   # Intro sequence video
│   └── images/                     # App icons/splash and misc images
├── components/                     # Reusable UI components + tests
├── constants/                      # Shared constants (e.g., theme colors)
├── hooks/                          # Shared hooks
├── modelLoader.ts                  # ONNX model load, tensor prep, inference, post-processing
├── app.json                        # Expo app config
├── eas.json                        # EAS build profiles
├── metro.config.js                 # Metro bundler config (.onnx support)
└── scripts/reset-project.js        # Expo scaffold reset helper
```

## Usage Example (Game Flow)

1. Launch the app and tap **START GAME**.
2. Allow camera/audio permissions.
3. App captures a frame and estimates number of players.
4. Confirm detected count or enter chair count manually.
5. Countdown starts, music plays, then pauses randomly.
6. Players sit; app decrements chair count each round.
7. Final round triggers winner detection and winner display.

## Contributing

1. Create a feature branch.
2. Install dependencies: `npm install`
3. Run checks before submitting:
   - `npm run lint`
   - `npm test -- --watchAll=false`
4. Keep changes scoped and include clear PR descriptions.

## Roadmap / Known Limitations

- Real-time high-FPS inference is not yet optimized (see `performance-analysis.md`).
- Winner detection relies on overlap heuristics and may fail in crowded/occluded scenes.
- Current detection thresholds and retry behavior may need device/environment tuning.
- Platform testing/optimization details for all device classes are still evolving.

## License

No license file is currently present in the repository.

**TODO:** Add a `LICENSE` file and update this section with the chosen license.
