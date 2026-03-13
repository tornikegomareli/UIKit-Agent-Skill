# UIKit Agent Skill for AI Coding Assistants

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![iOS 13–26](https://img.shields.io/badge/iOS-13–26-orange.svg)](https://developer.apple.com/ios/)
[![Platform: iOS](https://img.shields.io/badge/Platform-iOS-lightgrey.svg)](https://developer.apple.com/ios/)
[![Swift 6.2+](https://img.shields.io/badge/Swift-6.2+-F05138.svg)](https://swift.org)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-brightgreen.svg)](https://agentskills.io)

<img width="2816" height="1536" alt="Gemini_Generated_Image_kzkbtgkzkbtgkzkb" src="https://github.com/user-attachments/assets/bab2118f-d895-4e0d-8298-e41e84bdf36f" />


An agent skill that teaches AI coding assistants how to write correct, modern, and performant UIKit code. It covers the full surface of UIKit development from iOS 13 through iOS 26 — deprecated API replacement, the complete iOS 26 UIKit overhaul (Liquid Glass, updateProperties(), automatic Observable tracking, UICornerConfiguration, HDR colors, typed notifications, interruptible transitions, UIGlassEffect, UIScrollEdgeEffect, iPad menu bar, UISplitViewController inspectors), Auto Layout best practices, view controller lifecycle and architecture, diffable data sources and compositional layout, accessibility compliance, concurrency and thread safety, the #Preview macro, @Observable adoption patterns, and Objective-C interop. Every rule is annotated with its iOS version availability and includes fallback guidance for projects targeting older versions.

Built on the [Agent Skills](https://agentskills.io) open standard. Works with Claude Code, Codex, Gemini, Cursor, and any agent that supports the specification.

## What the Skill Covers

The skill performs a 12-step review of UIKit code, loading only the reference files relevant to the task:

| Domain | Key Topics |
|--------|------------|
| Deprecated API | UIAlertView, UIWebView, legacy cell properties, UIButton.Configuration, UIScene lifecycle mandatory migration, .done to .prominent, .inline to .integrated, all iOS 26 Liquid Glass button/glass/corner/HDR/notification APIs with fallbacks for iOS 13-25 |
| Views | UIView lifecycle including updateProperties() (iOS 26), composition vs inheritance, draw(_:) vs CALayer, UICornerConfiguration, .flushUpdates animation, automatic @Observable tracking in layoutSubviews/updateProperties/updateConstraints/draw with back-deployment to iOS 18, conditional observation gotchas, retain cycle risks |
| Auto Layout | Anchor API over NSLayoutConstraint(item:), NSLayoutConstraint.activate batching, UIStackView, content hugging/compression resistance, Safe Area, layout margins, scroll view contentLayoutGuide/frameLayoutGuide, NSDirectionalEdgeInsets for RTL |
| View Controllers | Full lifecycle order with viewIsAppearing (iOS 13+ back-deployed) and updateProperties (iOS 26+), separate view update cycle (Traits/updateProperties/layoutSubviews/Display), Massive VC avoidance, child containment API, coordinator pattern, UISplitViewController inspectors, UIHostingSceneDelegate, iPad menu bar with UIMainMenuSystem, Observable auto-tracking in 7+ VC methods |
| Navigation | Coordinator pattern, UISheetPresentationController detents (iOS 15+) with custom detents (iOS 16+), deep linking, UINavigationBarAppearance, iOS 26 subtitles/largeTitle/identifier/interruptible transitions/sharesBackground/tab bar minimize/UITabAccessory |
| Table/Collection Views | Diffable data sources (iOS 13+), compositional layout (iOS 13+), CellRegistration (iOS 14+), UIContentConfiguration (iOS 14+), UIHostingConfiguration for SwiftUI-in-cells (iOS 16+), configurationUpdateHandler (iOS 15+), reconfigureItems (iOS 15+), prefetching, Observable auto-tracking in cell providers (iOS 26+) |
| Accessibility | VoiceOver labels/traits/hints/values, Dynamic Type with UIFontMetrics and @ScaledMetric (iOS 14+), 44pt touch targets, Reduce Motion, color differentiation, accessibility notifications, grouped elements |
| Performance | Off-screen rendering avoidance, layer.shadowPath, layer.shouldRasterize, NSCache over Dictionary, UIImage.preparingForDisplay (iOS 15+) with CGImageSource fallback, main thread blocking detection, CADisplayLink over Timer, cell formatter reuse, Instruments profiling |
| Concurrency | @MainActor (iOS 13+ with Xcode 13.2+), Task cancellation in deinit/viewDidDisappear, actor isolation over locks, GCD-to-async/await migration patterns with code examples, Task.sleep(for:) (iOS 16+) vs nanoseconds fallback, withCheckedContinuation bridges, typed NotificationCenter.Message (iOS 26+) |
| Design | Dark Mode semantic colors, trait collections with registerForTraitChanges (iOS 17+) and traitCollectionDidChange fallback, SF Symbols progressive enhancement (iOS 13/15/16/17/26), Liquid Glass auto-adoption, UIGlassEffect, UIScrollEdgeEffect, HDR UIColor, UISlider tick marks/neutralValue/thumbless, UIView.LayoutRegion, UIBackgroundExtensionView |
| Swift | Modern Foundation APIs with version gates (replacing iOS 16+, FormatStyle iOS 15+, URL.documentsDirectory iOS 16+), Obj-C interop (@objc scoping, #selector, required init?(coder:)), Swift concurrency (async/await over GCD, strict Sendable, MainActor.run redundancy checks) |
| Code Hygiene | Weak delegates, NotificationCenter token-based observers, KVO with NSKeyValueObservation, Timer invalidation, Keychain over UserDefaults for secrets, #Preview macro (iOS 17+), @Observable (iOS 17+) with auto-tracking back-deploy to iOS 18, delegation/closures/Combine fallback for iOS 13-16 |

## Getting Started

The skill follows the [Agent Skills](https://agentskills.io) open standard, so it can be added to any compatible agent. Pick the method that fits your setup.

### Claude Code plugin install

Inside Claude Code, run:

```
/plugin add https://github.com/tornikegomareli/UIKit-Agent-Skill
```

This installs the skill as a plugin directly from the repository.

### One-line install

Works across Claude Code, Codex, Gemini, Cursor, and others:

```bash
npx skills add https://github.com/tgomareli/uikit-agent-skill --skill uikit-max
```

The installer will ask whether you want the skill available globally or scoped to a single project, and which agents on your machine should receive it.

Requires Node. If you don't have it: `brew install node` (macOS) or grab it from [nodejs.org](https://nodejs.org).

### Manual install

Clone the repo and drop the `uikit-max/` folder into whichever location your agent reads skills from. No build step, no dependencies — it is just Markdown files.

```bash
git clone https://github.com/tgomareli/uikit-agent-skill.git
cp -r uikit-agent-skill/uikit-max ~/.your-agent/skills/
```

Consult your agent's documentation for the exact skills directory path.

### Agent-specific setup

**Claude Code** — after installation, invoke with `/uikit-max` or ask naturally: "Use UIKit Max to review my project." Partial reviews work too: `/uikit-max Focus on accessibility and deprecated API.`

**Codex** — use `$uikit-max` to trigger the skill. Combine with instructions: `$uikit-max Check concurrency and thread safety.`

**Gemini / Cursor / Others** — the skill triggers automatically when the agent detects UIKit code in context, or you can reference it by name in your prompt.

In all cases, only the reference files relevant to the review are loaded, keeping token usage minimal.

## iOS Version Support

The skill is not limited to the latest iOS. Rules are annotated with their minimum iOS version, and the agent is instructed to respect the project's deployment target. When a modern API is not available at the target version, the skill provides the recommended fallback.

| iOS Version | What Becomes Available |
|-------------|----------------------|
| iOS 13 | UIScene, diffable data sources, compositional layout, context menus, UINavigationBarAppearance |
| iOS 14 | UICollectionView.CellRegistration, UIContentConfiguration, PHPickerViewController, UIAction initializers |
| iOS 15 | UIButton.Configuration, UISheetPresentationController, configurationUpdateHandler, reconfigureItems, preparingForDisplay |
| iOS 16 | UIHostingConfiguration, UICalendarView, custom sheet detents, URL.documentsDirectory, FormatStyle |
| iOS 17 | registerForTraitChanges, #Preview macro, @Observable, animated symbols, viewIsAppearing (public header) |
| iOS 18 | UITab API for UITabBarController |
| iOS 26 | Liquid Glass, updateProperties(), Observable auto-tracking, UICornerConfiguration, UIGlassEffect, HDR colors, typed notifications, interruptible transitions |

## Contributing

Adding new checks, correcting existing ones, or improving coverage of edge cases.

Guidelines:

- Keep Markdown concise. There is a token cost to using skills, so respect the budgets.
- Do not repeat things LLMs already know. Focus on edge cases, surprises, soft deprecations, and common LLM mistakes.
- All contributions must be licensed under MIT.

## License

UIKit Max is available under the [MIT License](LICENSE). See the LICENSE file for details.
