![preview](https://raw.githubusercontent.com/IMADDAALI/input-action-hud/main/banner_559cca.svg)
[![Download](https://raw.githubusercontent.com/IMADDAALI/input-action-hud/main/get_06408.svg)](https://IMADDAALI.github.io/input-action-hud/)

# 🎮 Control Hints — Dynamic Input Action UI for Roblox

**Control Hints** is a Roblox module that reads the modern **Input Action System** and automatically renders a clean, adaptive control-hints panel for your players. Instead of hand-drawing a keyboard graphic every time you remap a key, you declare your actions once — and the module paints the interface for you, whether your player is on a laptop, a gamepad, or a touchscreen.

![License](https://img.shields.io/badge/license-MIT-blue)
![Platform](https://img.shields.io/badge/platform-Roblox-00A2FF)
![Language](https://img.shields.io/badge/language-Luau-6E4AFF)
![Status](https://img.shields.io/badge/status-actively%20maintained-brightgreen)
![Responsive](https://img.shields.io/badge/UI-responsive-orange)
![Localization](https://img.shields.io/badge/i18n-40%2B%20locales-purple)
![Build](https://img.shields.io/badge/build-passing-success)
![Contributions](https://img.shields.io/badge/contributions-welcome-ff69b4)

---

## 🧭 Table of Contents

- [Overview](#-overview)
- [Why Control Hints Exists](#-why-control-hints-exists)
- [Core Concepts](#-core-concepts)
- [Feature List](#-feature-list)
- [How It Works](#-how-it-works)
- [Quick Start](#-quick-start)
- [Configuration Options](#-configuration-options)
- [Layout Strategies](#-layout-strategies)
- [Multilingual Support](#-multilingual-support)
- [Accessibility & Responsive UI](#-accessibility--responsive-ui)
- [Theming & Styling](#-theming--styling)
- [API Reference](#-api-reference)
- [Events & Signals](#-events--signals)
- [Performance Notes](#-performance-notes)
- [Common Recipes](#-common-recipes)
- [Roadmap](#-roadmap)
- [SEO & Discoverability](#-seo--discoverability)
- [Community & Support](#-community--support)
- [Disclaimer](#-disclaimer)
- [License](#-license)

---

## 🌟 Overview

Every great Roblox experience teaches its players how to move, jump, aim, dodge, build, and interact. The trouble is that **control prompts age badly**. The moment you rename an action, swap a keybind, or slot in a new input device, your carefully placed hint labels become misleading. Players press the wrong button, get frustrated, and log off.

**Control Hints** solves that problem by treating player input as a living, queryable source of truth. It hooks directly into the **Input Action System**, listens for binding changes, and reflows the on-screen prompt tray in real time.

Think of it as a signal tower for your game's controls: each action broadcasts what device it belongs to, and the module lights up exactly the right beacon for the player in front of the screen.

The project is deliberately unopinionated. You bring your action definitions, your preferred visual language, and your layout preferences. The module handles detection, grouping, glyph resolution, spacing, transitions, and localization fallbacks — the fiddly parts nobody wants to rewrite for every project.

---

## ❓ Why Control Hints Exists

Roblox developers have historically solved control prompts in one of three ways, each with its own pain points:

- **Hardcoded labels.** A `TextLabel` that says "Press E to interact." Works until the player changes their bindings, then becomes a lie.
- **Device branching.** A messy `if` ladder that checks `UserInputService.GamepadEnabled` and builds a different frame for every platform. Fragile and duplicated.
- **Third-party mockups.** Static UI designed in an external tool, which can never reflect a player's personal remaps at all.

Control Hints replaces all three with a single pipeline: **declare → detect → render → react**. You describe your input map once, and the module keeps the visible hints synchronized with reality for the entire session.

---

## 🧠 Core Concepts

Before diving into code, it helps to understand four ideas the module is built around.

**1. Actions, not keys.**
You never tell the module that "the jump button is A." You tell it that a `Jump` action exists and let the Input Action System report which glyph represents it right now.

**2. Device profiles.**
The same action can present differently across keyboard/mouse, gamepad, and touch. Control Hints tracks these as interchangeable *profiles* and switches automatically when the active input device changes.

**3. Hint groups.**
Rather than one long list, hints are organized into named groups like `Movement`, `Combat`, or `Inventory`. Groups can be shown, hidden, reordered, or collapsed independently.

**4. Reactive rendering.**
The panel is not built once and forgotten. It subscribes to binding updates and repaints the affected slots only, keeping the cost of a rebind event negligible.

---

## ✨ Feature List

- 🎯 **Automatic glyph resolution** — reads bindings straight from the Input Action System.
- 📱 **Device-aware rendering** — detects keyboard, gamepad, and touch and switches layouts.
- 🧩 **Grouped hint trays** — organize actions into collapsible, named categories.
- 🌍 **Multilingual support** — 40+ bundled locale strings with graceful fallback to English.
- 🎨 **Full theming surface** — colors, corner radius, padding, fonts, and animation curves are all configurable.
- ♿ **Accessibility-first** — respects reduced-motion preferences and offers high-contrast palettes.
- ⚡ **Lightweight by design** — no per-frame polling; updates are event-driven.
- 🧠 **Smart rebind tracking** — hints follow player remaps live, without a reload.
- 📐 **Responsive UI** — the tray reflows for small phones, wide monitors, and everything between.
- 🔔 **Signal API** — subscribe to `HintShown`, `HintHidden`, and `DeviceChanged` events.
- 🧪 **Deterministic testing hooks** — simulate a device profile for automated UI tests.
- 🕐 **Around-the-clock customer support** — our community channel is staffed continuously across time zones.
- 🔁 **Hot-swappable themes** — change the entire look at runtime with a single call.
- 🪝 **Zero global side effects** — safe to drop into existing projects of any size.
- 📦 **Modular imports** — pull in only the renderer, only the detector, or the whole kit.
- 🧭 **Layout presets** — corner-stack, bottom-bar, radial, and compact-icon arrangements.
- 🧬 **Extensible glyph providers** — plug in custom icon sets for consoles or peripherals.
- 💾 **Session persistence** — remember a player's preferred tray position across visits.
- 🧵 **Thread-safe updates** — queue a rebind burst and the module coalesces it into one paint.

---

## ⚙️ How It Works

At a high level, the module runs through a short, repeatable loop:

1. **Registration.** You hand Control Hints a table describing your actions and how they should be grouped.
2. **Detection.** A lightweight sensor layer watches for the first meaningful input and categorizes it as a device profile.
3. **Resolution.** For each registered action, the module queries the current binding and asks a glyph provider to convert it into a visual token.
4. **Rendering.** The resolved tokens are placed into the active layout strategy and parented to a screen container.
5. **Reaction.** When a binding changes or a new device takes over, only the affected slots are repainted.

Because every stage is isolated, you can replace any one of them without disturbing the others — a custom glyph provider, a bespoke layout strategy, or your own detection heuristic.

---

## 🚀 Quick Start

Getting a first hint tray on screen is intentionally a short journey.

**Step one — bring the module into your project.**
Place the Control Hints package inside your project's shared module area so both client scripts and build tooling can reach it.

**Step two — declare your actions.**
Create a small configuration table. Each entry names an action, assigns it to a group, and optionally overrides its label or ordering.

**Step three — initialize on the client.**
In a LocalScript, require the module and call its initializer with your configuration. The module will detect the device, resolve glyphs, and build the tray.

**Step four — let it breathe.**
From that point on, the module maintains itself. You can nudge it, theme it, or dismantle it, but you never have to babysit it.

A typical consumer of the module looks like this in prose: the LocalScript gathers the player's saved preference for tray position, passes the action config into the initializer, subscribes to the `DeviceChanged` signal to log analytics, and then does nothing else for the rest of the session.

---

## 🛠️ Configuration Options

The configuration table is where your project's personality shines through. The most commonly tuned keys are:

- **`position`** — anchor the tray to any corner or edge.
- **`orientation`** — vertical stack, horizontal bar, or grid.
- **`density`** — how much horizontal breathing room each hint receives.
- **`showLabels`** — toggle between icon-only and icon-plus-text modes.
- **`animation`** — choose the enter/exit transition curve, or disable motion entirely.
- **`theme`** — supply a palette table or reference a bundled theme by name.
- **`locale`** — override the auto-detected language for controlled testing.
- **`groups`** — an ordered list describing which hint groups appear and in what sequence.
- **`glyphProvider`** — swap in a custom resolver for unique iconography.
- **`rememberPosition`** — persist the player's chosen tray corner between sessions.

Every option has a documented default, so an empty table is a perfectly valid configuration.

---

## 🗺️ Layout Strategies

Control Hints ships with several ready-made arrangements, each tuned for a different kind of experience:

- **Corner Stack** — the classic column of hints tucked into a screen corner. Best for exploration and sandbox titles.
- **Bottom Bar** — a full-width strip of prompts near the lower edge, ideal for action and shooter games.
- **Radial Fan** — hints arranged around the crosshair for fast-twitch competitive play.
- **Compact Icons** — glyphs only, minimal chrome, maximum information density.
- **Contextual Drawer** — a hint tray that reveals itself on demand and retreats when the player is busy.

Custom layout strategies can be authored by implementing a small interface that receives a list of resolved hint tokens and returns positioned frames.

---

## 🌍 Multilingual Support

Language should never be the reason a player feels lost. Control Hints bundles a broad set of locale strings and resolves them in this order:

1. An explicit locale you set in the configuration.
2. The locale reported by the platform's localization service.
3. English as the ultimate fallback.

To add a language, drop a new string table into the locale directory following the existing naming convention. Missing keys fall back silently, so partial translations are always safe to ship and refine over time.

---

## ♿ Accessibility & Responsive UI

Interfaces should welcome everyone. The module includes:

- **Reduced-motion handling** — respects a player's motion preference and swaps animated transitions for instant swaps.
- **High-contrast palettes** — two bundled themes designed for legibility against busy scenes.
- **Scalable glyph sizing** — text and icons scale with the player's UI preference.
- **Responsive reflow** — hints reposition automatically when the viewport changes, including when players rotate their devices.
- **Screen-reader-friendly labels** — hidden textual descriptions accompany every glyph.

---

## 🎨 Theming & Styling

Themes are plain tables, which means they can be built, loaded from a datastore, or generated at runtime. Each theme defines:

- Background and border colors.
- Corner radius and stroke thickness.
- Text color, font family, and weight.
- Padding values for the tray and for individual hints.
- Motion durations and easing curves.

Because theming is data-driven, a game can offer players a light theme for bright daytime sessions and a muted theme for late-night play — all with one call.

---

## 📚 API Reference

The public surface is intentionally compact. The module exposes methods for initialization, mutation, and teardown.

- **`ControlHints.new(config)`** — construct a manager instance bound to your configuration.
- **`manager:start(container)`** — begin detection and rendering into the given container.
- **`manager:stop()`** — detach listeners and remove rendered hints.
- **`manager:setTheme(theme)`** — hot-swap the active palette.
- **`manager:setLayout(name)`** — switch layout strategies at runtime.
- **`manager:setLocale(code)`** — change the active language.
- **`manager:registerActions(actions)`** — add actions after initialization.
- **`manager:showGroup(name)`** — reveal a specific hint group.
- **`manager:hideGroup(name)`** — hide a specific hint group.
- **`manager:refresh()`** — force a full repaint, useful after bulk changes.
- **`manager:destroy()`** — fully dispose of the manager and its resources.

Each method returns the manager where chaining makes sense, enabling fluent setup chains.

---

## 🔔 Events & Signals

Reactive games benefit from listening to the module rather than polling it. Available signals include:

- **`DeviceChanged`** — fires when the active input device switches.
- **`HintShown`** — fires when a hint becomes visible, including its group name.
- **`HintHidden`** — fires when a hint is removed or hidden.
- **`GroupToggled`** — fires when a named group is expanded or collapsed.
- **`ThemeApplied`** — fires after a theme swap completes.

Signals are compatible with the conventional Roblox connection style, so cleanup is straightforward.

---

## ⚡ Performance Notes

Control Hints avoids the classic performance traps:

- No per-frame loops in steady state.
- Binding updates are coalesced so bursts result in a single paint.
- Frames are pooled and reused across repaints rather than recreated.
- Hidden groups do not consume layout computation.

In practice, an idle tray costs effectively nothing, and an active rebind storm settles within a frame or two.

---

## 🧪 Common Recipes

- **Show combat hints only during combat.** Toggle the `Combat` group alongside your state machine.
- **Give touch players a larger tray.** Detect the device and apply a denser configuration on mobile.
- **Offer a theme picker.** Persist the chosen theme per player and apply it on join.
- **Localize on the fly.** React to the platform's language change signal and call `setLocale`.
- **Test on a fake device.** Use the testing hooks to force a gamepad profile during automated runs.

---

## 🗓️ Roadmap

Planned improvements for the year 2026 and beyond:

- Handheld console glyph packs.
- A visual configuration editor companion.
- Snapshot-based regression screenshots for CI.
- Additional layout strategies voted on by the community.
- Expanded locale coverage with community contributions.
- Optional integration examples for popular UI frameworks.

---

## 🔍 SEO & Discoverability

If you are searching for **Roblox control hints**, **Input Action System UI**, **dynamic keybind display for Roblox**, **gamepad prompt rendering**, or **multilingual game UI modules**, this project is built precisely for those needs. The module is frequently described as a **responsive Roblox input hint panel**, a **device-aware controls overlay**, and a **reactive keybind display module** — all accurate summaries of what it does. Natural, descriptive naming is used throughout the documentation so that developers and search engines alike understand the intent.

---

## 🤝 Community & Support

Questions, ideas, and pull requests are all welcome. Because contributors span many time zones, our community channel maintains **around-the-clock customer support** — there is almost always someone awake to help. When filing an issue, include your device profile, the actions you registered, and the layout strategy in use, and the problem is usually diagnosable within minutes.

Contribution guidelines favor small, focused changes with clear reasoning. New layouts, locales, and glyph providers are the most requested areas, and first-time contributors are actively encouraged.

---

## ⚠️ Disclaimer

This module is an independent, community-built utility and is not affiliated with, endorsed by, or sponsored by Roblox Corporation. "Roblox" and related marks are the property of their respective owners and are referenced here only for descriptive purposes. The software is provided **as is**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability arising from the use of the software. You are responsible for ensuring that your use complies with the platform's terms of service and any applicable guidelines. Always test thoroughly before shipping to a live player base.

---

## 📄 License

This project is released under the **MIT License**. You are welcome to use, modify, merge, publish, distribute, sublicense, and sell copies of the software, provided that the original copyright notice and permission notice are included.

Read the full license text here: [MIT License](https://opensource.org/licenses/MIT)

Copyright (c) 2026 Control Hints contributors.

---

[![Download](https://raw.githubusercontent.com/IMADDAALI/input-action-hud/main/get_06408.svg)](https://IMADDAALI.github.io/input-action-hud/)