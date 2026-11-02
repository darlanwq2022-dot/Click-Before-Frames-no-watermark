# v2.99.8 — Tick Tuning

- New Tick Tune menu tab with independent per-mode minimum and maximum physics-tick bounds.
- Reference defaults: Min 1 tick on tunable modes; Ball Max 79 ticks; Cube, UFO, Robot, Spider and Swing Max Off. Ship and Wave are N/A and keep their dedicated timing logic.
- Tick values use the 240-Hz reference scale; values are converted at unlocked physics rates.
- Max value tap toggles Off ↔ 80 ticks; +/- controls change the bound by one tick.
- Quick presets: Min=1 all, Max=80 all, Max Off all.
- Search bounds account for mode, stage, low-latency, old per-mode frames, and no-hitch resume elapsed time.
- Orb-alignment candidate search respects the minimum bound as well.
- Diagnostic export prints each mode's min/max tick values and marks Ship/Wave N/A.
