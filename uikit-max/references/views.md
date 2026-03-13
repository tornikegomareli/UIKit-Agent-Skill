# UIView Lifecycle, Composition, and Custom Drawing

- Always call `super` in lifecycle overrides: `layoutSubviews()`, `updateProperties()`, `draw(_:)`, `didMoveToSuperview()`, `didMoveToWindow()`, `traitCollectionDidChange(_:)`.
- Override `draw(_:)` only when Core Graphics drawing is truly necessary. Prefer `CALayer` properties (`backgroundColor`, `cornerRadius`, `borderWidth`, `borderColor`) and sublayers for static visuals.
- Set `translatesAutoresizingMaskIntoConstraints = false` on every view before adding programmatic Auto Layout constraints. Missing this causes constraint conflicts with the autoresizing mask.
- Prefer composition (adding subviews) over deep subclass hierarchies. Extract reusable UI into self-contained `UIView` subclasses.
- Use block-based `UIView.animate(withDuration:animations:)`. Never use legacy `beginAnimations(_:context:)` / `commitAnimations()`.
- Understand `frame` vs `bounds`: use `bounds` for internal layout of subviews, `frame` for positioning within superview.
- Use `UIView.transition(with:duration:options:animations:completion:)` for simple cross-dissolve or flip transitions.
- Flag `layoutSubviews()` overrides that do not call `super.layoutSubviews()`.
- Avoid storing strong references to superview or sibling views. Navigate the view hierarchy or use delegation patterns.
- Use `@IBInspectable` / `@IBDesignable` only when Interface Builder is actively used in the project.
- Prefer `CAGradientLayer` over `draw(_:)` for simple gradient backgrounds.
- Custom views with a natural size should override `intrinsicContentSize` to cooperate with Auto Layout. Call `invalidateIntrinsicContentSize()` when the natural size changes.
- Use `UIView.performWithoutAnimation {}` to suppress implicit animations when updating constraints or layer properties inside an animation block.
- For rounded corners, use `layer.cornerRadius` with `layer.maskedCorners` to round specific corners instead of a mask layer.
- Prefer `UIView.animateKeyframes(withDuration:delay:options:animations:completion:)` for multi-step sequential animations over nesting completion handlers.

## iOS 26 View Updates

- Use `updateProperties()` for property and styling updates that should run before layout. Override it to separate property hydration from layout logic. Trigger manually with `setNeedsUpdateProperties()` (iOS 26+).
- The update pass order is: Traits → `updateProperties()` → `layoutSubviews()` → Display Pass. Use `updateProperties()` for data/style binding; reserve `layoutSubviews()` for geometry.
- Use `UIView.cornerConfiguration` with `UICornerConfiguration` for corner rounding instead of `layer.cornerRadius` + `layer.maskedCorners`. Supports per-corner radii, `.capsule()`, and `.containerConcentric(minimum:)` for nested corner radius calculation (iOS 26+).
- Use `.flushUpdates` animation option with `UIView.animate(withDuration:options:)` to eliminate manual `layoutIfNeeded()` calls in animation blocks. UIKit automatically flushes pending updates before and after the animation (iOS 26+).
- UIKit now tracks `@Observable` objects automatically in these `UIView` methods: `layoutSubviews()`, `updateProperties()`, `updateConstraints()`, `draw(_:)`, and `configurationUpdateHandler` / `updateConfiguration(using:)` on cells. No manual `setNeedsLayout()` or `withObservationTracking` required (iOS 26+, back-deployable to iOS 18 via `UIObservationTrackingEnabled` Info.plist key).
- Observation is conditional: only properties actually accessed during the method execution create dependencies. If a property is behind an `if` branch that doesn't execute, it won't be tracked — this is efficient by design.
- Gotcha: observable objects are retained while being observed. Architect carefully to prevent retain cycles — use `[weak self]` patterns and avoid storing closures that capture both the observable and the view.
- Avoid pre-accessing all observable properties upfront to "register" them — this creates unnecessary dependencies and defeats the tracking system's efficiency.
- Cache expensive computations derived from observable properties, as tracked methods may re-run frequently during layout cycles.
