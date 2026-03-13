# Navigation Controllers, Coordinators, and Modal Presentation

- For complex navigation, prefer the coordinator pattern to decouple view controllers from navigation logic. View controllers should communicate via delegates or closures, not push/present other controllers directly.
- Use `pushViewController(_:animated:)` / `popViewController(animated:)` for standard navigation stack flow. Avoid directly manipulating the `viewControllers` array unless implementing custom transitions.
- Set `modalPresentationStyle` and `modalTransitionStyle` before calling `present(_:animated:completion:)`.
- Configure `UISheetPresentationController` detents (iOS 15+) — `.medium()`, `.large()`, and custom detents (iOS 16+) — for half-sheet presentation instead of building custom slide-up views. On iOS 13–14, use `modalPresentationStyle = .pageSheet` without detent customization.
- Flag `performSegue(withIdentifier:sender:)` usage in programmatic-UI projects — prefer direct instantiation and presentation.
- For deep linking, centralize route parsing in the coordinator or in `UISceneDelegate.scene(_:openURLContexts:)` and `scene(_:continue:)`.
- Prefer `UITabBarController` configuration via `UITab` items (iOS 18+) over legacy `viewControllers` array assignment when targeting modern iOS.
- Flag circular strong references between coordinators and view controllers. Coordinators hold strong references to children; children hold `weak` references to coordinators and delegates.
- Use `navigationItem` configuration (title, `largeTitleDisplayMode`, bar button items) in `viewDidLoad()`, not in the parent controller.
- Prefer `UINavigationBarAppearance` (iOS 13+) and `UIToolbarAppearance` for customizing navigation bar styling over direct property manipulation.
- For modal flows requiring their own navigation stack, embed the presented view controller in a `UINavigationController`.

## iOS 26 Navigation Updates

- Use `UINavigationItem.subtitle`, `.attributedSubtitle`, or `.subtitleView` for displaying subtitles in the navigation bar (iOS 26+).
- Use `UINavigationItem.largeTitle` and `.largeSubtitle` for customizing large title display independently from the standard title (iOS 26+).
- Use `UINavigationItem.identifier` to control navigation transition behavior — matching identifiers produce cross-fade transitions instead of push/pop (iOS 26+).
- Use `UINavigationItem.interactiveContentPopGestureRecognizer` to enable swipe-to-dismiss from anywhere in the content area, not just the screen edge (iOS 26+).
- Navigation transitions are now interruptible in iOS 26 — users can interact with content without waiting for push/pop animations to complete.
- Use `UIBarButtonItem.sharesBackground = true` to group adjacent bar button items under a shared Liquid Glass background (iOS 26+).
- Use `UINavigationBarAppearance.subtitleTextAttributes` and `.largeSubtitleTextAttributes` for styling navigation bar subtitles (iOS 26+).
- Use `UITabBarController.tabBarMinimizeBehavior` (`.onScrollDown`, `.onScrollUp`, `.never`) to control tab bar auto-minimize behavior during scrolling (iOS 26+). Use `contentLayoutGuide` to adapt content to tab bar size changes.
- Use `UITabAccessory` to add a media player toolbar or custom accessory view above the tab bar (iOS 26+).
