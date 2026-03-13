# Off-Screen Rendering, Layer Optimization, Image Caching, and Main Thread Performance

## Layer and Rendering

- Avoid off-screen rendering in scrolling contexts: flag `cornerRadius` combined with `masksToBounds` / `clipsToBounds` on views inside `UIScrollView`, `UITableView`, or `UICollectionView`. Prefer pre-rendering rounded corners in images or using `CAShapeLayer` masks.
- Set `layer.shouldRasterize = true` (with `layer.rasterizationScale = UIScreen.main.scale`) only for complex, static layer hierarchies. Never rasterize frequently-changing content — it defeats the purpose.
- Always set `layer.shadowPath` explicitly when applying shadows. Without it, Core Animation computes the shadow path from the layer's contents every frame.
- Use `layer.maskedCorners` to round specific corners instead of a full mask layer.
- Prefer `setNeedsDisplay()` over direct `draw(_:)` calls to coalesce redraws within the same run loop cycle.

## Images

- Cache images with `NSCache`, not `Dictionary`. `NSCache` automatically evicts under memory pressure.
- Decode and downsample images on a background thread. Use `UIImage.preparingForDisplay()` (iOS 15+) for decompression, or `CGImageSource` with `kCGImageSourceThumbnailMaxPixelSize` for downsampling large images. On iOS 13–14, use `CGImageSource` for both decompression and downsampling.
- For large image lists, downsample to the display size rather than loading full-resolution images into memory.
- Flag `UIColor(patternImage:)` with large images — it tiles without downsampling and can consume excessive memory.

## Main Thread

- Never block the main thread with network requests, JSON parsing, image decoding, or Core Data fetches. All must run off-main.
- Avoid `layoutIfNeeded()` in tight loops or scroll view delegate callbacks — it forces immediate layout passes.
- Use `CADisplayLink` for frame-synchronized updates instead of `Timer` with small intervals.

## Table and Collection View Performance

- Set `estimatedRowHeight` / `estimatedItemSize` to a reasonable value to reduce layout passes during initial load and scrolling.
- Ensure cell heights are consistent or cached when using manual height calculation.
- Cancel in-flight image downloads in `prepareForReuse()` to prevent stale images appearing in reused cells.
- Avoid creating `DateFormatter`, `NumberFormatter`, or other formatters inside `cellForRowAt` — create them once and reuse.
- Profile with Instruments (Time Profiler, Core Animation, Allocations) before guessing at performance problems.
