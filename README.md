# StudentWatch

[![Swift](https://img.shields.io/badge/Swift-5.0-F05138?style=flat&logo=swift&logoColor=white)](https://swift.org/)
[![UIKit](https://img.shields.io/badge/UIKit-iOS-2396F3?style=flat&logo=apple&logoColor=white)](https://developer.apple.com/documentation/uikit)
[![iOS | iPadOS](https://img.shields.io/badge/iOS%20%7C%20iPadOS-15.0%2B-000000?style=flat&logo=apple&logoColor=white)](https://developer.apple.com/ipados/)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue?style=flat)](LICENSE)

StudentWatch (shipped under the display name E-Monitor) is an iPad and iPhone app for exam invigilation. A single screen lays out 16 desks with two seats each, so a proctor can give a different amount of time to any of 32 students, watch every countdown at a glance, and get an alert the moment a student's time runs out. It is aimed at teachers and exam proctors who run individually timed or accommodated exams in one room. The on-screen interface is in Turkish, and the app is at the development stage.

## Features

- Individual countdown timers for 32 students: enter the exam length in minutes and the label counts down live
- Remaining time for every student visible at a glance
- Alert when a student's time is up, after which that seat resets to idle
- Per-student reset button to stop and clear a running timer
- 16 tables of two seats, all visible on one iPad screen, with scrolling on phones, smaller iPads and narrow windows
- Input validation (whole minutes from 1 to 1440) and deadline-based countdowns that stay correct after time in the background; timers live in memory and do not survive app termination

## Tech stack

| Layer | What is used |
| --- | --- |
| Language / UI | Swift 5.0, UIKit (programmatic views and Auto Layout, only `LaunchScreen.storyboard`) |
| Timing and alerts | `Timer` added to the main run loop in `.common` mode (via `RunLoop.main.add(_:forMode:)`), `UIAlertController` |
| Assets | SF Symbols (`studentdesk`, `arrow.counterclockwise`) and asset-catalog colour sets |
| Dependencies | None (no CocoaPods, Carthage or Swift Package Manager packages) |

## Getting started

### Prerequisites

- macOS with Xcode 27 or later
- iOS or iPadOS 15.0 or later on a simulator or device (test targets require iOS 17 or later)

### Installation

1. Clone the repository and open the project.

   ```bash
   git clone https://github.com/CanDuru4/student-watch.git
   open student-watch/StudentWatch.xcodeproj
   ```

2. Select the shared `StudentWatch` scheme and an iPad or iPhone simulator, or a connected device.
3. For a device, under Signing & Capabilities replace the checked-in development team with your own and change the bundle identifier (`com.CanDuru.StudentWatch`) to one you own.
4. Run (⌘R).

No API keys, `.env` file or environment variables are required; the app is entirely offline and stores nothing outside the running process.

## Project structure

```
StudentWatch/
├── AppDelegate.swift              App lifecycle, scene configuration
├── SceneDelegate.swift            Builds the window, sets the classroom screen as root
├── ClassOneViewController.swift   Classroom screen: 32 timers, layout, actions
├── CountdownTime.swift            Minute validation and deadline-based remaining time
├── Assets.xcassets/               App icon and the AppColor1-3 colour sets
├── Base.lproj/LaunchScreen.storyboard
└── Info.plist
StudentWatch.xcodeproj/            Xcode project to open
StudentWatchTests/                 XCTest unit tests for countdown logic and layout
StudentWatchUITests/               XCUITest input, countdown and rotation flow
Logo/                              E-Monitor logo files and brand board
docs/assets/                       README images
CHANGELOG.md                       Version history
```

## Testing

Run the `StudentWatch` scheme's tests from Xcode (⌘U). Unit tests cover minute validation, deadline-based countdowns across suspension, and the classroom layout; the UI test covers invalid input, a running countdown and rotation. There is no CI.

## Screenshots

<p align="center">
  <img src="docs/assets/screenshot-ipad.png" alt="StudentWatch classroom screen on iPad" width="600">
</p>

## License

MIT. See [LICENSE](LICENSE).

## Author

Can Duru — [canduru.net](https://canduru.net)
