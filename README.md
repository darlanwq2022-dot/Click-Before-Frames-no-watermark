# Predictive Input Buffer v3.0.2 — native physics and wave reliability

Targets Geometry Dash 2.2081 / Geode 5.11.2.

## v3.0.2 build fix

- Fixed the compile error in `alignOrb`: the resumed-search lower bound (`searchFrom`) is now passed explicitly into the orb-alignment routine instead of being referenced before its declaration.
- Marked the popup `init()` method as `override`, removing the corresponding Clang warning.
- No gameplay behavior, physics timing, or prediction limits were changed by this build-only patch.

## What changed in v3.0.1

- Removed the mod's custom physics accumulator and `getModifiedDelta` override. Geometry Dash 2.2 owns its native 240-TPS fixed physics loop; the mod now calls the original game update exactly once and measures render cadence separately.
- The predictor advances using the actual `PlayerObject::update(dt)` delivered by the game. Its 240-Hz tick windows are converted using the measured physics-step duration; render FPS is not used to scale prediction time or CPU budget.
- Added render FPS and physics-steps-per-render-frame to diagnostic exports to make timing regressions visible.
- Raised the default wave lookahead to 240 native physics ticks (1 second) and the configured prediction budget to 16 ms.
- No-hitch searches now use 1 ms bounded slices and can resume across up to 1024 physics updates. New inputs remain queued in order while the original prediction is resumed.
- Robust saves, wave safety margin, CBF sync and no-hitch continuation are enabled by the v3.0.1 migration. Low Latency and Performance Mode are disabled for the high-coverage wave profile.
- Old saved wave horizons below 160 are migrated to 240. Old prediction budgets below 8 ms are raised to 16 ms. Higher user values are preserved.

## FPS vs TPS

For Geometry Dash 2.2, the normal physics clock is 240 TPS; this is not the same as the render FPS cap. Setting the mod to 60 TPS would not mean "60 FPS" and would change the simulation timing. This version does not create a custom TPS loop: it leaves the game physics alone.

At 30 rendered FPS, the game can still process multiple native physics steps per rendered frame. However, the instant at which an input reaches the game may still be constrained by input sampling. Click Between Frames / Click On Steps integration can improve input placement; the buffer cannot reconstruct an earlier physical click timestamp that never reached its hook. The diagnostic log reports whether CBF is detected and whether CBF sync is enabled.

## Wave profile

The default horizon is 240 ticks. A larger lookahead and 16 ms budget give the wave search more opportunities to evaluate delayed candidates. This is not a guarantee that every gap has a safe correction: `UNBUFFERED` is still correct when no tested candidate survives the configured robustness checks. Larger searches can also defer later inputs while pending; keep an eye on the exported `BUDGET YIELD`, `RESUME`, `UNBUFFERED` and `DIRECT FALLBACK` counts.

## Build

Use the included GitHub Actions workflow or compile against the matching Geode SDK. The SDK is not installed in the authoring environment, so this source package does not include a compiled `.geode` binary. Static validation can be performed locally, but a successful Geode compile and in-game test are still required.
