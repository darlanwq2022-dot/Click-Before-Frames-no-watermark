# Predictive Input Buffer v2.99.7 — Clean UI

A clean-interface fork of the supplied Predictive Input Buffer 2.99.5 source.

## What changed
- Removed the gameplay watermark, auto-input tag, training mode/hint, path visualization, and progress-bar heatmap.
- Removed the in-game console panel, its hotkey, and the optional Windows live-console window.
- Added a clear Buffer ON/OFF switch that remembers the last active stage.
- Added quick presets: Low delay, Balanced, and More saves.
- Exposed smart prediction, full player-state copying, and automatic mode helpers directly in the in-game menu.
- Reorganized the Tools page so the console cannot be opened over gameplay; diagnostic logs can still be copied/exported from the menu.
- Kept the prediction and input-buffer core logic, saved settings, and original source attribution.

## Menu
Open the menu in a level with the configured menu key (default: **7**). It is also available from the main-menu shortcut and pause menu.

## Build
Build with the Geode SDK configured by the project. This source package has **not** been compiled in this environment because the Geode SDK is not installed here.

## Attribution
Modified from the original project: https://github.com/raresdalas1009/-predictive-input-buffer


## Reliability / diagnostics fork

This fork keeps prediction searches resumable across physics ticks, preserves later input events while a search is pending, and records input decisions, budget yields, resumes, and direct fallbacks in the in-memory diagnostic log. Use the in-level **Copy log** button (or the configured export key, F8 by default) after a test run and share `console-export.txt` when reporting an issue.

The mod cannot guarantee that every click can be corrected: some inputs have no verified safe delay, and prediction is disabled for unsupported situations (for example, platformer mode or disabled buffering). In those cases it delivers the input and records the reason rather than silently dropping it. **Low latency** is off by default because it intentionally caps the delay window and CPU budget.

## v2.99.7 reliability changes

Custom windows now default to 128 physics ticks (32 frames), can be raised to 256 ticks (64 frames), and the per-mode slider reaches 32 frames. Rapid-input bypass and failed-spot memory blacklisting are opt-in, so they do not bypass prediction by default. Auto-budget reduces a detected hitch to 8 ms instead of 4 ms.
