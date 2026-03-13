# Human Interface Guidelines, Dark Mode, Trait Collections, and Adaptive Layout

## Colors and Theming

- Support Dark Mode: use semantic colors (`UIColor.label`, `UIColor.systemBackground`, `UIColor.secondaryLabel`, `UIColor.separator`, `UIColor.systemFill`) instead of hardcoded `.black` / `.white`.
- Use asset catalog colors with "Any Appearance" and "Dark" variants for custom brand colors.
- Flag hardcoded `UIColor` RGB values that do not adapt to appearance changes.

## Typography

- Prefer `UIFont.preferredFont(forTextStyle:)` over hardcoded font sizes for HIG compliance and Dynamic Type support.
- Test layouts with the largest accessibility content size categories to ensure text does not clip or overlap.
- For custom fonts, always wrap with `UIFontMetrics` to support Dynamic Type scaling.

## Layout and Adaptivity

- Use `traitCollection.horizontalSizeClass` and `.verticalSizeClass` for adaptive layouts. Flag device-specific checks (`UIDevice.current.userInterfaceIdiom == .pad`) that could be replaced with trait-based conditions.
- Respond to trait changes using `registerForTraitChanges(_:handler:)` (iOS 17+). On iOS 13–16, override `traitCollectionDidChange(_:)` instead.
- Ensure minimum 44×44pt touch targets for all interactive elements per Apple HIG.
- Avoid hardcoded padding, spacing, or frame sizes unless explicitly requested. Prefer `directionalLayoutMargins` and system spacing.

## System Components and Styling

- Use SF Symbols via `UIImage(systemName:)` (iOS 13+) with `UIImage.SymbolConfiguration` for weight, scale, and color consistency. Hierarchical rendering (iOS 15+), variable values (iOS 16+), and automatic animated symbols (iOS 17+) add progressive enhancement.
- Use `UIListContentConfiguration.cell()`, `.subtitleCell()`, `.valueCell()` for standard list cell appearance.
- Use `UINavigationBarAppearance` / `UITabBarAppearance` / `UIToolbarAppearance` for bar customization instead of directly setting bar properties.
- Use `UIAppearance` proxy for global styling when many instances need the same customization, but prefer `UIContentConfiguration` for per-element styling.
- Use `UIColor.tintColor` or `view.tintColor` for the app's accent color rather than hardcoding a specific color.

## iOS 26 Design Updates

- **Liquid Glass**: Apps compiled with Xcode 26 automatically adopt the Liquid Glass design system. Standard UIKit components (navigation bars, tab bars, search fields, alerts, popovers) gain translucent, dynamic material effects automatically.
- Use `UIGlassEffect(style:)` (`.regular` or `.clear`) with `UIVisualEffectView` for applying Liquid Glass to custom views. Set `isInteractive = true` for tap-expand behavior (iOS 26+).
- Use `UIScrollEdgeEffect` on scroll views to configure content fading under Liquid Glass navigation bars. Styles: `.automatic`, `.soft`, `.hard` (iOS 26+).
- Use `UIColor(red:green:blue:alpha:exposure:)` for HDR colors that auto-adjust to display capabilities. Use `.standardDynamicRange` fallback for SDR contexts (iOS 26+).
- Use `UIColorPickerViewController.maximumLinearExposure` to enable HDR color selection with a Boost slider (iOS 26+).
- SF Symbols 7: use `.gradient` color rendering mode via `UIImage.SymbolConfiguration(colorRenderingMode: .gradient)` for automatic gradient generation. Use `.draw` variable value mode for animated appearance effects (iOS 26+).
- Use `UIButton.Configuration.symbolContentTransition` with `UISymbolContentTransition.replace(.downUp)` for animated symbol transitions in buttons (iOS 26+).
- `UISlider` enhancements: use `.sliderStyle` (`.default`, `.thumbless`), `TrackConfiguration` for tick marks, `neutralValue` for anchor points, and `enabledRange` for clamping (iOS 26+).
- Use `UIView.LayoutRegion` (`.safeArea(cornerAdaptation:)`, `.margins(cornerAdaptation:)`) with `layoutGuide(for:)` for layout regions that adapt corner radius automatically (iOS 26+).
- Use `UIBackgroundExtensionView` to extend content visually into unsafe areas under Liquid Glass sidebar platters (iOS 26+).
