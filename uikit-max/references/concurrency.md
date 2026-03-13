# Main Thread Safety, async/await, and DispatchQueue Migration

- All UI updates must occur on the main thread. Flag any `UIView` or `UIViewController` property mutation called from a background context.
- Prefer `@MainActor` annotation on view controller classes and UI-updating methods over manual `DispatchQueue.main.async` calls. `@MainActor` is available from iOS 13+ when building with Xcode 13.2 or later (Swift concurrency runtime is embedded in the app). On older Xcode toolchains, use `DispatchQueue.main.async`.
- For legacy codebases using GCD, flag `DispatchQueue.main.sync` called from the main thread — this causes a deadlock.
- When migrating from GCD to Swift concurrency, replace:
  ```swift
  // Before
  DispatchQueue.global().async {
      let result = self.process()
      DispatchQueue.main.async {
          self.updateUI(result)
      }
  }

  // After
  Task {
      let result = await process()
      await MainActor.run { updateUI(result) }
  }
  ```
- Use `Task` and structured concurrency for view controller work. Cancel tasks in `deinit` or `viewDidDisappear(_:)` to avoid updating a deallocated controller.
- Store `Task` handles as properties and call `.cancel()` during cleanup to prevent work from completing after the view controller is dismissed.
- Flag `NotificationCenter` observation blocks that capture `self` strongly and update UI without checking for deallocation. Use `[weak self]`.
- For background data processing, prefer `actor` isolation over manual `NSLock`, `os_unfair_lock`, or `DispatchQueue` synchronization.
- Prefer `Task.sleep(for:)` with `Duration` (iOS 16+) over `Task.sleep(nanoseconds:)`. On iOS 13–15, `Task.sleep(nanoseconds:)` is the only option.
- Prefer `async`/`await` overloads of system APIs (URLSession, CoreData, etc.) over their closure-based equivalents. Swift concurrency requires iOS 13+ (runtime) but full `async`/`await` API availability varies — e.g., `URLSession.data(from:)` requires iOS 15+. On iOS 13–14, use completion handler APIs with `withCheckedContinuation` bridges if needed.

## iOS 26 Notifications

- Use strongly typed `NotificationCenter.Message` types instead of parsing `userInfo` dictionaries. For example, `NotificationCenter.default.addObserver(for: .keyboardWillShow) { message in let frame = message.keyboardFrame }` — no casting required (iOS 26+).
