# v3.0.3 — Wave timing and FPS diagnostics

- Fixed the overly strict Wave search introduced in v3.0.0: individual Wave inputs are now judged against a local survival window (baseline + 12 native physics ticks), rather than requiring one isolated toggle to survive the entire 225/240-tick horizon.
- Added learned Wave press-hold and release-to-press timing. When a real timing sample exists, the ghost predicts the next opposite toggle; subsequent real inputs remain queued with the same shift to preserve the input rhythm.
- Kept narrow verified Wave windows usable. Adjacent safe timings are used to centre a wider window when available, not as a prerequisite for accepting a narrow one.
- Clarified diagnostic output: configured total prediction budget vs current no-hitch slice budget.
- Replaced the one-render-frame physics-step sample with a rolling 120-frame average, so high render FPS no longer misleadingly reports zero as the average physics cadence.
- Kept the Geometry Dash native physics clock untouched; this is not a forced TPS change.
