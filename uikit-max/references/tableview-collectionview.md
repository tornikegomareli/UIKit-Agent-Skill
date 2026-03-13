# UITableView, UICollectionView, Diffable Data Sources, and Compositional Layout

## Prefer Modern APIs

- Prefer `UICollectionView` with `UICollectionLayoutListConfiguration` (iOS 14+) over `UITableView` for new code. Collection views with list configuration provide the same appearance with more layout flexibility. On iOS 13, use `UITableView` or `UICollectionViewFlowLayout`.
- Prefer `UICollectionViewDiffableDataSource` / `UITableViewDiffableDataSource` (iOS 13+) over manual `numberOfRows`/`cellForRow` implementations for any non-trivial data.
- Apply snapshots on the main queue. Use `apply(snapshot, animatingDifferences:)` for UI updates.
- Use `UICollectionViewCompositionalLayout` (iOS 13+) instead of `UICollectionViewFlowLayout` for any layout beyond a simple single-column list.
- For simple lists, use `UICollectionLayoutListConfiguration` (iOS 14+) with `UICollectionViewCompositionalLayout.list(using:)`.

## Cell Registration and Configuration

- Use modern cell registration: `UICollectionView.CellRegistration` (iOS 14+) and `dequeueConfiguredReusableCell(using:for:item:)` instead of string-based `register`/`dequeueReusableCell(withReuseIdentifier:)`. On iOS 13, use string-based registration.
- Use `UIContentConfiguration` / `UIListContentConfiguration` (iOS 14+) for cell content instead of directly setting deprecated `cell.textLabel` / `cell.imageView` properties. On iOS 13, the legacy cell properties are still available.
- Use `configurationUpdateHandler` (iOS 15+) on cells for state-driven appearance updates (highlighted, selected, disabled).
- Prefer `reconfigureItems(_:)` (iOS 15+) over `reloadItems(_:)` in diffable data sources when only cell content changes but identity remains the same. On iOS 13–14, use `reloadItems(_:)`.

## Performance and Data

- Implement `UICollectionViewDataSourcePrefetching` / `UITableViewDataSourcePrefetching` for image-heavy or network-dependent cells.
- Never perform expensive work (network calls, disk I/O, complex computation) inside `cellForItemAt` / `cellForRowAt`. Prepare data before the cell is requested.
- Flag cells that hold state which could bleed between reuses (active image downloads, timers) but do not implement `prepareForReuse()` to reset it.
- For self-sizing cells, ensure the content view's Auto Layout constraints fully define the cell's height from top to bottom.
- Set `estimatedRowHeight` / `estimatedItemSize` to a reasonable value to reduce initial layout passes.

## Advanced Patterns

- Use `UIHostingConfiguration` (iOS 16+) to embed SwiftUI views directly inside `UICollectionViewCell` or `UITableViewCell`. Cells are self-resizing by default when SwiftUI content changes. On iOS 13–15, use `UIHostingController` embedded as a child view controller in the cell.
- Use `UICollectionView.SectionSnapshot` (iOS 14+) for section-level data management with outline/expandable sections.
- For swipe actions, configure `UISwipeActionsConfiguration` via the diffable data source's or delegate's `trailingSwipeActionsConfigurationForRowAt` method.
- Use `UICollectionView.SupplementaryRegistration` (iOS 14+) for section headers and footers instead of string-based supplementary view registration.

## iOS 26 Updates

- UIKit now tracks `@Observable` objects used inside diffable data source cell provider closures and `configurationUpdateHandler` blocks, automatically reconfiguring cells when observed properties change — while the cell is visible. No manual snapshot reapply or `reconfigureItems` needed (iOS 26+, back-deployable to iOS 18 via `UIObservationTrackingEnabled` Info.plist key).
- Use `.flushUpdates` animation option when applying snapshots inside `UIView.animate` blocks — UIKit automatically flushes pending Observable-driven updates without manual `layoutIfNeeded()` calls (iOS 26+).
