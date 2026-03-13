# Memory Management, Retain Cycles, KVO/Notification Cleanup, and deinit Patterns

- Delegates must be declared `weak` to prevent retain cycles. Flag any `var delegate: SomeDelegate` missing `weak`.
- Remove `NotificationCenter` observers in `deinit`. Prefer the token-based `addObserver(forName:object:queue:using:)` API that returns a removable `NSObjectProtocol` token. Store the token and call `NotificationCenter.default.removeObserver(token)` in `deinit`.
- Remove KVO observations in `deinit`. Prefer modern `observe(_:options:changeHandler:)` with stored `NSKeyValueObservation` tokens over legacy `addObserver(_:forKeyPath:options:context:)`.
- Invalidate `Timer` instances in `deinit` or `viewWillDisappear(_:)` to prevent retain cycles and leaked timers. `Timer.scheduledTimer` retains its target.
- Never store secrets (API keys, tokens, passwords) in the repository. Use the Keychain for sensitive data at runtime. Never use `UserDefaults` for passwords or tokens.
- Use `[weak self]` in closures passed to long-lived objects (notification observers, network callbacks, timers) to prevent retain cycles. Use `[unowned self]` only when the closure's lifetime is guaranteed to be shorter than `self`.
- Code comments should be present where logic is not self-evident. Use documentation comments (`///`) on public API.
- Unit tests should exist for core business logic. UI tests only where unit tests are not feasible.
- If the project uses SwiftLint, ensure zero warnings and errors.
- Use `Localizable.xcstrings` for localization. Prefer symbol keys (`"profile.title"`) over natural-language keys.
- Use `#Preview` macro (iOS 17+) for previewing `UIViewController` and `UIView` in Xcode without running the simulator. Do not use the legacy `PreviewProvider` protocol — it requires more boilerplate and is deprecated in favor of the macro. Add `@available(iOS 17, *)` if the project's minimum target is below iOS 17.
- Use `@Observable` classes (iOS 17+ / Swift 5.9+) for shared state instead of KVO, `NotificationCenter`, or delegate callbacks for model-to-view communication. On iOS 26+, UIKit tracks observable properties automatically in lifecycle methods. Back-deploy automatic tracking to iOS 18 by adding `UIObservationTrackingEnabled = YES` to Info.plist. On iOS 13–16, use delegation, closures, or Combine for model-to-view updates.
