# Modern Swift Code and Objective-C Interop

## Modern Swift

- Use Swift-native string methods: `replacing(_:with:)` (iOS 16+) not `replacingOccurrences(of:with:)`. On iOS 15 and earlier, `replacingOccurrences(of:with:)` is fine.
- Use modern Foundation API: `URL.documentsDirectory` (iOS 16+), `.appending(path:)` (iOS 16+) instead of `FileManager.default.urls(for:in:)`. On iOS 15 and earlier, use `FileManager` API.
- Prefer `FormatStyle` APIs (iOS 15+): `value.formatted(.number.precision(.fractionLength(2)))` over C-style `String(format:)`. On iOS 14 and earlier, `String(format:)` or `NumberFormatter` are acceptable.
- Prefer static member lookup (`.circle` not `Circle()`) where available.
- Avoid force unwraps (`!`) and force try (`try!`). Use `if let`, `guard let`, nil-coalescing (`??`), `try?`, or `do-catch`.
- For text filtering and search, use `localizedStandardContains(_:)` not `contains(_:)` — it handles diacritics, case, and locale correctly.
- Prefer `Double` over `CGFloat` in Swift code, except where an API specifically requires `CGFloat` (optionals or `inout` parameters).
- Use `count(where:)` instead of `filter { }.count`.
- Use `Date.now` over `Date()`.
- Use `if let value {` shorthand instead of `if let value = value {`.
- Omit `return` for single-expression functions and computed properties.
- Flag silent error swallowing: `catch { print(error) }` or `catch { print(error.localizedDescription) }` — errors should be handled or reported meaningfully.
- Structs repeatedly sorted with the same closure should conform to `Comparable` instead.
- Use `PersonNameComponents` and `PersonNameComponentsFormatter` for names instead of string interpolation.
- Use `"y"` not `"yyyy"` for year format strings — `"yyyy"` does not handle week-based year edge cases correctly.

## Objective-C Interop

- Prefer Swift-native types (`String`, `Array`, `Dictionary`) over `NSString`, `NSArray`, `NSDictionary`. Bridge explicitly only when the API requires it.
- Use `@objc` only on members that must be visible to the Objective-C runtime: `#selector` targets, KVO-observed properties, `@IBAction`/`@IBOutlet`. Do not blanket-apply `@objc` to entire classes.
- Prefer `#selector(methodName)` syntax over string-based selectors.
- When subclassing Objective-C classes (`UIViewController`, `UIView`, `UITableViewCell`), implement both the designated initializer and `required init?(coder:)`.
- Use `@objc enum` only when exposing to Objective-C code. Prefer Swift-native enums with associated values otherwise.

## Swift Concurrency

- Prefer `async`/`await` over closure-based asynchronous APIs.
- Prefer Swift concurrency (`@MainActor`, `MainActor.run {}`, `Task`) over GCD (`DispatchQueue.global().async`, `DispatchQueue.main.async`) in new code. GCD is acceptable for projects that must support Xcode versions prior to 13.2.
- Prefer `Task.sleep(for:)` with `Duration` (iOS 16+) over `Task.sleep(nanoseconds:)`. On iOS 13–15, `Task.sleep(nanoseconds:)` is acceptable.
- Flag mutable shared state not protected by an actor or `Sendable` conformance.
- Assume strict concurrency checking is enabled. Flag `@Sendable` closure violations.
- Check whether `MainActor.run {}` calls are actually necessary — if the enclosing function is already `@MainActor`, the call is redundant.
- Avoid `Task.detached` unless truly needing to escape the current actor context.
