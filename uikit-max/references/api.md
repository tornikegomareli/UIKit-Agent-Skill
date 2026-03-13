# Deprecated APIs and Modern Replacements

- Use `UIAlertController` with `.alert` or `.actionSheet` style instead of `UIAlertView` or `UIActionSheet`.
- Use `WKWebView` instead of `UIWebView`. On iOS 26+, prefer SwiftUI's native `WebView` if embedding in a SwiftUI host.
- Use `UISearchController` instead of `UISearchDisplayController`.
- Use `UIContextMenuInteraction` (iOS 13+) instead of deprecated `UIPreviewInteraction` or 3D Touch peek/pop.
- Use `UIWindowScene`-based APIs (iOS 13+) instead of `UIApplication.shared.windows` or `UIApplication.shared.keyWindow`. Access the window via `view.window?.windowScene`.
- Use `PHPickerViewController` (iOS 14+) instead of `UIImagePickerController` for photo/video selection. On iOS 13, `UIImagePickerController` is still the only option.
- Use `UIContentConfiguration` / `UIListContentConfiguration` (iOS 14+) instead of `cell.textLabel`, `cell.detailTextLabel`, and `cell.imageView` — these properties are deprecated. On iOS 13, the legacy cell properties are still available.
- Use `viewIsAppearing(_:)` for geometry-dependent setup instead of `viewWillAppear(_:)` when the view's final bounds are needed. Back-deployed to iOS 13+.
- Use `UIButton.Configuration` (iOS 15+) — `.filled()`, `.plain()`, `.gray()`, `.tinted()`, `.bordered()`, `.borderedProminent()` — instead of manual `setTitle`, `setTitleColor`, `contentEdgeInsets`. On iOS 13–14, use the traditional `setTitle(_:for:)` / `setImage(_:for:)` API.
- Use `UICalendarView` (iOS 16+) for date selection instead of building custom calendar views. On iOS 15 and earlier, use `UIDatePicker` with `.datePickerMode`.
- Use `configurationUpdateHandler` (iOS 15+) on `UIButton`, `UICollectionViewCell`, and `UITableViewCell` for state-driven appearance updates.
- Flag any usage of `performSelector(onMainThread:)` — use `@MainActor` or `MainActor.run {}`.
- Use `UIScene`/`UISceneDelegate` lifecycle (iOS 13+) instead of `UIApplicationDelegate` methods like `applicationDidEnterBackground`. Per-scene state belongs in `UISceneDelegate`. Starting after iOS 26, apps that have not adopted the scene lifecycle will not launch.
- Use `UIBarButtonItem.Style.prominent` instead of the deprecated `.done` style (iOS 26+).
- Use `UINavigationBarAppearance.prominentButtonAppearance` and `UIToolbarAppearance.prominentButtonAppearance` instead of the deprecated `doneButtonAppearance` (iOS 26+).
- Use `UINavigationItem.SearchBarPlacement.integrated` instead of the deprecated `.inline` (iOS 26+). New options include `.integratedButton` and `.integratedCentered`.
- Use `UIAction`-based initializers (iOS 14+) for `UIButton`, `UIBarButtonItem`, and `UIControl` targets instead of `addTarget(_:action:for:)` with `#selector` where possible. On iOS 13, use `addTarget(_:action:for:)`.
- Use `UIMenu` (iOS 14+ for bar buttons, iOS 13+ for context menus) for contextual menus and bar button items instead of `UIActionSheet` or custom popover menus.
- Prefer `UISheetPresentationController` detents (iOS 15+) over custom slide-up views. On iOS 13–14, use `modalPresentationStyle = .pageSheet` without detent customization.
- Flag `UIApplication.shared.open(_:options:completionHandler:)` with empty options — pass `[:]` explicitly or use the simpler `UIApplication.shared.open(_:)` overload.

## iOS 26 Additions

- Use `UIButton.Configuration.glass()`, `.clearGlass()`, `.prominentGlass()`, or `.prominentClearGlass()` for Liquid Glass button styles (iOS 26+).
- Use `updateProperties()` and `setNeedsUpdateProperties()` for property updates that should run before layout, without triggering a full layout pass (iOS 26+). Available on both `UIView` and `UIViewController`.
- Use `UIView.cornerConfiguration` (`UICornerConfiguration`) instead of manual `layer.cornerRadius` + `layer.maskedCorners` for corner rounding. Supports `.capsule()`, `.corners(radius:)`, and per-corner radii (iOS 26+).
- Use `UIGlassEffect` for applying Liquid Glass material to custom views. Set `isInteractive = true` for tap-expand behavior and `tintColor` for stained-glass effects (iOS 26+).
- Use `UIBarButtonItem.Badge` (`.count(_:)`, `.string(_:)`, `.indicator()`) for adding badges to bar button items (iOS 26+).
- Use `UIHostingSceneDelegate` protocol to host SwiftUI scenes inside UIKit apps for incremental SwiftUI adoption (iOS 26+).
- Use strongly typed `NotificationCenter.Message` types instead of `userInfo` dictionary casting for keyboard and other system notifications (iOS 26+).
- Use `UIColor(red:green:blue:alpha:exposure:)` for HDR color support. Use `UIColorPickerViewController.maximumLinearExposure` to enable HDR color picking (iOS 26+).
- Use `UINavigationItem.subtitle`, `.attributedSubtitle`, or `.subtitleView` for navigation bar subtitles. Use `.largeTitle` and `.largeSubtitle` for large title display (iOS 26+).
- Use `UINavigationItem.identifier` to control navigation transition behavior between view controllers (iOS 26+).
- Use `UIScrollEdgeEffect` (`.soft`, `.hard`) on scroll views for graceful content fading under Liquid Glass navigation bars (iOS 26+).
- Use `UIBackgroundExtensionView` to surface content under sidebar glass platters in split view layouts (iOS 26+).
- Apps compiled with Xcode 26 automatically adopt Liquid Glass. Use `UIDesignRequiresCompatibility = YES` in Info.plist to temporarily opt out. This flag will be removed in Xcode 27.
