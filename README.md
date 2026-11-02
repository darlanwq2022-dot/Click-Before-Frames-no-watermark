# Predictive Input Buffer — Clean UI

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
