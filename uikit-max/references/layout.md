# Auto Layout, Anchors, UIStackView, and Safe Area

- Prefer the layout anchor API (`topAnchor.constraint(equalTo:)`) over `NSLayoutConstraint(item:attribute:relatedBy:toItem:attribute:multiplier:constant:)`.
- Always batch constraint activation with `NSLayoutConstraint.activate([...])` instead of setting `isActive = true` individually. Batching is more efficient and avoids intermediate invalid states.
- Prefer `UIStackView` for linear arrangements (horizontal or vertical) before resorting to manual constraints. Set `axis`, `spacing`, `alignment`, and `distribution` explicitly.
- Set `contentHuggingPriority` and `contentCompressionResistancePriority` explicitly when labels, buttons, or text fields compete for space. The defaults are often not what you want.
- Constrain content to `safeAreaLayoutGuide` for areas that must avoid the notch, home indicator, status bar, and navigation/tab bars.
- Constrain content to `layoutMarginsGuide` for content that should respect system-standard margins.
- Avoid hardcoded `CGRect` frame assignments in `viewDidLoad` or `viewWillLayoutSubviews` when Auto Layout is in use. Mixing frames and constraints causes unpredictable behavior.
- Every view with Auto Layout needs enough constraints to determine both position (x, y) and size (width, height). Flag ambiguous layouts.
- When constraints cannot all be satisfied, adjust `UILayoutPriority` values rather than breaking constraints. Use `.defaultHigh` (750) and `.defaultLow` (250) rather than arbitrary numbers.
- When updating constraints dynamically, prefer modifying the `constant` property of a stored `NSLayoutConstraint` reference over deactivating and reactivating constraints.
- For self-sizing table/collection view cells, set `estimatedRowHeight` (or `estimatedItemSize`) and `rowHeight = UITableView.automaticDimension`. Ensure the cell's content views define a complete vertical constraint chain.
- Avoid `updateConstraints()` / `setNeedsUpdateConstraints()` unless implementing batched constraint updates for complex animations. For most cases, update constraints directly.
- Use `UIStackView.addArrangedSubview(_:)` and `removeArrangedSubview(_:)` — not plain `addSubview` / `removeFromSuperview` — to manage stack view children.
- For equal spacing or distribution, set `UIStackView.distribution = .equalSpacing` or `.fillEqually` rather than adding manual spacer views.
- Use `NSDirectionalEdgeInsets` (leading/trailing) instead of `UIEdgeInsets` (left/right) for layout margins to support right-to-left languages.
- For scroll views, constrain the content view's edges to the scroll view's `contentLayoutGuide` and width to the `frameLayoutGuide` for vertical scrolling.
