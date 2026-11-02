# Predictive Input Buffer v3.0.4 — Wave diagnostic build fix

Targets Geometry Dash 2.2081 / Geode 5.11.2.

## v3.0.4 — Wave diagnostic build fix

The Android/Clang build exposed three declaration-order errors in the diagnostic export: `exportConsole()` referenced the learned Wave press-hold, release-gap, and next-toggle state before those definitions appeared in the translation unit. The export now calls three small accessors declared before `exportConsole()` and implemented after the corresponding state is defined. This preserves the diagnostic values while fixing the compile error. The `MenuLayer::init()` hook is also explicitly marked `override`.

No Wave scoring, input scheduling, physics cadence, or runtime buffering behavior changed in this build-fix release.

## v3.0.3 — fix for the reported DIOOZ GOD behavior

The v3.0.2 log showed that the mod was active but Wave rejected nearly every candidate. The previous v3 wave search required a single isolated toggle to survive the full 225/240-tick lookahead. That is not a sound criterion for continuous Wave steering: the player's next opposite input is part of the actual control sequence and is normally queued with the same delay. A candidate could improve the immediate gap yet be rejected because the simulated button was held for almost a second.

- Wave search now scores a local danger window (baseline survival + 12 physics ticks) instead of requiring one button state to survive the entire lookahead.
- If the next opposite Wave input has been observed before, the predictor learns the player's press-hold duration and release-to-press gap, then includes that next toggle in the ghost simulation.
- The real follow-up input is still queued with the original delay, preserving the player's press/release rhythm.
- If no timing sample exists yet, the first press uses a conservative one-physics-tick hold estimate; release-to-press timing is not guessed until a real cycle is observed.
- Wave timing margin now prefers the centre of a contiguous safe timing window, but still accepts a verified narrow window rather than rejecting it for lacking adjacent safe delays.
- The full wave horizon remains available for trajectory/recovery checks; it is no longer used as the minimum survival requirement for every individual input.
- Diagnostic output distinguishes per-search-slice budget from the configured budget.
- Physics steps per rendered frame are now reported as a rolling average over the last 120 rendered frames. A single zero-step render frame at high FPS is normal and is no longer shown as the average.

## What the v3.0.1 native-clock change means

Geometry Dash 2.2 owns its native 240-TPS physics loop. The mod does not force 60, 240 or 960 TPS and does not use render FPS to scale prediction time. At 30 rendered FPS the game can process several physics steps per rendered frame; at 480 rendered FPS many rendered frames can contain zero physics steps while the average remains around 240 physics updates per second.

Click Between Frames / Click On Steps integration can improve the timestamp at which an input reaches the game. The buffer cannot reconstruct a physical click timestamp that never reaches its input hook.

## Wave profile and limits

The default wave horizon is 240 native physics ticks. The maximum delay setting controls how far the mod may move an input; it does not guarantee that a safe timing exists. The local wave scoring criterion is intended to make the buffer useful across successive press/release events without pretending that one toggle can steer through a whole section alone.

`UNBUFFERED` means the simulator did not verify a useful timing within the tested delay window. It is distinct from `DIRECT FALLBACK`, which is a search interruption caused by budget/resume limits. Keep the exported `wave`, `SAVED`, `UNBUFFERED`, `BUDGET YIELD` and `RESUME` diagnostics when reporting a failed gap.

## Build

Current source version: **3.0.4**. Use the included GitHub Actions workflow or compile against the matching Geode SDK. The SDK is not installed in the authoring environment, so this source package does not include a compiled `.geode` binary. Static validation can be performed locally, but a successful Geode compile and in-game test are still required.
