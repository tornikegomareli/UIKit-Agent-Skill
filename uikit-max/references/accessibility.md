# VoiceOver, Dynamic Type, Accessibility Traits

- Set `accessibilityLabel` on all interactive elements that lack visible text: icon-only buttons, image views used as buttons, custom controls.
- Set `accessibilityTraits` appropriately: `.button` for tappable non-`UIButton` views, `.header` for section headers, `.adjustable` for slider-like custom controls, `.image` for meaningful images.
- Support Dynamic Type: use `UIFont.preferredFont(forTextStyle:)` and set `adjustsFontForContentSizeCategory = true` on `UILabel`, `UITextField`, and `UITextView`.
- For custom fonts with Dynamic Type, use `UIFontMetrics.default.scaledFont(for: customFont)`.
- Use `@ScaledMetric` (available from iOS 14) for scaling spacing, icon sizes, and other non-font metrics with the user's preferred content size.
- Respect `UIAccessibility.isReduceMotionEnabled` — replace complex or large animations with simple fades or no animation.
- Respect `UIAccessibility.isDifferentiateWithoutColorEnabled` — add icons, patterns, or labels where color alone conveys meaning.
- Group related elements using `accessibilityElements` array or `shouldGroupAccessibilityChildren = true` on container views to improve VoiceOver navigation.
- Post `UIAccessibility.Notification.screenChanged` or `.layoutChanged` after significant UI updates (modal appearance, content refresh) so VoiceOver re-reads the screen.
- Ensure minimum 44×44pt touch targets for all interactive elements per Apple Human Interface Guidelines.
- Flag custom controls that do not implement `accessibilityActivate()` or set appropriate traits for VoiceOver interaction.
- Set `accessibilityHint` for non-obvious actions — describe what will happen, not what the element is.
- Use `accessibilityValue` for controls with state (toggles, sliders, steppers) to communicate current value.
