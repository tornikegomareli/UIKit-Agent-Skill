# UIViewController Lifecycle, Container Controllers, and Architecture

- Respect the full UIViewController appearance lifecycle order: `loadView()` → `viewDidLoad()` → `viewWillAppear(_:)` → `viewIsAppearing(_:)` (back-deployed to iOS 13+) → `viewDidAppear(_:)` → `viewWillDisappear(_:)` → `viewDidDisappear(_:)`. Separately, the view update cycle runs: Traits → `updateProperties()` (iOS 26+) → `layoutSubviews()` → Display. On iOS 13–25, place property updates in `viewWillLayoutSubviews()` or `viewIsAppearing(_:)` instead of `updateProperties()`.
- Never perform layout calculations or geometry-dependent setup in `viewDidLoad()` — the view's bounds are not yet final. Use `viewIsAppearing(_:)` or `viewDidLayoutSubviews()`.
- Override `loadView()` only when building the entire view hierarchy programmatically without a storyboard or XIB. When overriding, do **not** call `super.loadView()`.
- Flag view controllers that access `self.view` in `init` — this triggers premature view loading.
- Keep `viewDidLoad()` focused on one-time setup (adding subviews, configuring constraints, setting delegates). Recurring updates belong in `viewWillAppear(_:)` or trait change callbacks.

## Avoiding Massive View Controller

- Extract business logic into dedicated model or service objects. View controllers should coordinate between the view and the model, not contain business rules.
- Use the coordinator pattern or flow controllers for navigation logic — view controllers should not know how to present other view controllers.
- Delegate data source and delegate logic to dedicated objects when the implementation is non-trivial (`UITableViewDataSource`, `UICollectionViewDataSource`).
- Prefer dependency injection (initializer or property) over singletons for providing data and services to view controllers.

## Child View Controller Containment

- When adding a child view controller, always follow the full containment API in order:
  1. `addChild(child)`
  2. `view.addSubview(child.view)` (and set up constraints)
  3. `child.didMove(toParent: self)`

- When removing a child view controller:
  1. `child.willMove(toParent: nil)`
  2. `child.view.removeFromSuperview()`
  3. `child.removeFromParent()`

- Flag code that adds a child controller's view without calling the containment API — appearance callbacks will not fire correctly.
- Prefer view controller composition (container controllers) over inheritance for sharing behavior across screens.

## iOS 26 Controller Updates

- Override `updateProperties()` on `UIViewController` to update properties and styling before layout, without triggering a layout pass. Use `setNeedsUpdateProperties()` to schedule an update (iOS 26+).
- `UISplitViewController` now supports inspector columns (iOS 26+). Configure with `minimumInspectorColumnWidth`, `preferredInspectorColumnWidth`, and `maximumInspectorColumnWidth`. Columns are resizable by dragging separators.
- Use `UISplitViewController.isShowing(_:)` to check column visibility, and respond to `splitViewController(_:didShow:)` / `splitViewController(_:didHide:)` delegate callbacks (iOS 26+).
- Use `UIHostingSceneDelegate` to host SwiftUI scenes inside a UIKit app for incremental adoption of SwiftUI views and visionOS features (iOS 26+).
- UIKit now tracks `@Observable` objects automatically in these `UIViewController` methods: `updateProperties()`, `viewWillLayoutSubviews()`, `viewDidLayoutSubviews()`, `updateViewConstraints()`, and `updateContentUnavailableConfiguration(using:)`. Also tracked in `UIPresentationController`: `containerViewWillLayoutSubviews()` and `containerViewDidLayoutSubviews()` (iOS 26+, back-deployable to iOS 18 via `UIObservationTrackingEnabled` Info.plist key).
- Use `UIMainMenuSystem` and `UIMainMenuSystemConfiguration` for configuring the iPad menu bar (iOS 26+). Use `UIDeferredMenuElement.usingFocus(identifier:)` for dynamic, context-aware menu population from the responder chain.
