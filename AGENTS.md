# StudentWatch (E-Monitor)

iPad/iPhone exam-invigilation app: one screen of 16 desks x 2 seats, each with its own countdown timer for 32 students. Personal portfolio project (modernized in 2026-09, PR #4). Swift 5, UIKit (programmatic), no third-party dependencies. Public repo, MIT.

## Repo map

- `StudentWatch/ClassOneViewController.swift` - the whole UI (~3.4k lines). Defines `ClassViewController` (the file name and its header comment do not match the class name). Holds all 32 seats, the scroll-view viewport, constraints, and actions.
- `StudentWatch/CountdownTime.swift` - pure logic: minute validation (1...1440) and deadline-based remaining seconds. Put new timing logic here, not in the view controller.
- `StudentWatch/SceneDelegate.swift` - builds the window with `ClassViewController()` as root. `AppDelegate.swift` is template lifecycle only.
- `StudentWatch/Assets.xcassets` - app icon and `AppColor1`-`AppColor3` colour sets. `Base.lproj/LaunchScreen.storyboard` is the only storyboard.
- `StudentWatchTests/` - XCTest unit tests (`CountdownTime`, layout overlap/scroll checks, one check per student label).
- `StudentWatchUITests/` - XCUITest flow: invalid input, running countdown, rotation, scrolling to student 32; plus launch performance.
- `design-assets/` - manual brand and design assets: `logo/` (svg/pdf/png), `source/` (brand board), `fonts/`, `banner/`, `screenshots/` (README images). `CHANGELOG.md` - version history (stale, 2023).

## Commands

The Xcode project is at the repo root (`StudentWatch.xcodeproj`); single shared scheme `StudentWatch`, configs Debug/Release.

```bash
xcodebuild -list -project StudentWatch.xcodeproj
xcodebuild build -project StudentWatch.xcodeproj -scheme StudentWatch -destination 'generic/platform=iOS Simulator'
xcodebuild test -project StudentWatch.xcodeproj -scheme StudentWatch -destination 'platform=iOS Simulator,name=<installed iPad or iPhone>'
xcodebuild test -project StudentWatch.xcodeproj -scheme StudentWatch -destination '<same>' -only-testing:StudentWatchTests
```

- Tests need an installed iOS simulator runtime (`xcrun simctl list runtimes`); unit/UI test targets require iOS 17+.
- No CI, no linter config, no deploy pipeline, no App Store release flow in the repo.

## Conventions and gotchas

- Seats are hand-duplicated, not data-driven. Table k uses properties named with the spelled-out number (`tableOne...`, `timerOnelabel`, `timeOne`) and seat 2 appends `2` to the number word (`tableOne2...`, `timerOne2label`, `timerLabelOne2Function`). Student number = 2k-1 for seat 1, 2k for seat 2. Any behaviour change to a seat must be applied to all 32 blocks (vars, `setUpVc`, constraints, `timerXAction` / `timerXResetAction` / `timerLabelXFunction`).
- Seat properties and `timerLabel*Function` / `timer*ResetAction` are internal (not private) because unit tests drive them via `@testable import`. Do not make them private.
- All per-seat timing goes through `startCountdown(...)`, which computes from a fixed deadline and adds the timer to `RunLoop.main` in `.common` mode. Keep countdowns deadline-based so time spent suspended is not lost. Timers are in memory only; nothing persists.
- The classroom is laid out at a minimum of 1032 x 1040 pt inside a `UIScrollView`; smaller windows scroll. `testNarrowClassroomKeepsCentralControlsSeparate` guards this.
- The UI is in Turkish, and UI tests find elements by those exact strings (for example `Öğrenci 1: Süreyi Ayarla`, `Sınav Süresi`, `Tamam`, `Geçersiz süre`, `Sıra 1 (Öğrenci 1-2)`). Changing copy means updating the tests.
- Deployment targets: app iOS 15.0, test targets iOS 17.x. Display name `E-Monitor`, bundle id `com.CanDuru.StudentWatch`, device family iPhone + iPad.
- No secrets, API keys, or env vars exist; the app is fully offline. Keep it that way in this public repo.

## Docs

- `README.md` - features, setup (signing: replace the checked-in team and bundle id for device builds), project structure, testing notes.
