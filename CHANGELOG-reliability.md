# v2.99.7 — Expanded buffer window and fewer bypasses

- Custom delay windows are raised from 64 to 128 240-Hz ticks (16 to 32 frames) on first load; the new settings support up to 256 ticks (64 frames).
- Per-mode delay sliders now support up to 32 frames instead of 10.
- The adaptive CPU-budget cap during a detected frame hitch is raised from 4 ms to 8 ms. The cap remains adaptive to avoid compounding a severe hitch.
- Rapid-input bypass is now an opt-in setting, OFF by default. Fast taps continue through prediction and the normal queue path unless the user enables the guard.
- Failed-spot memory blacklisting is now opt-in, OFF by default. The mod still learns and reuses successful delays; it simply tries a fresh search at previously problematic spots.
- Diagnostic exports now show whether rapid-input bypass and failed-spot blacklisting are enabled. Safe no-op decisions are labelled `PASS-THROUGH`; cases with no safe solution are labelled `UNBUFFERED`, so these are not confused with CPU-budget fallbacks.
- The resumable-search ceiling is raised to 512 continuation steps for the expanded window.

## Reading the supplied diagnostic

The previous export showed `16f / wave 16f`, which is 64 internal 240-Hz physics ticks, not 16 ticks. It also showed 10 budget yields and 10 resumes, zero direct fallbacks, 15 failed-spot blacklists, one rapid-input bypass, 236 saved inputs, 121 safe pass-through decisions, and three inputs for which no safe delayed solution was verified. The adaptive budget was 4 ms at export time despite a configured 30 ms, consistent with its frame-hitch cap.

This version targets the repeated level-memory and rapid-input bypasses and expands the delay window. It does not force unsafe delays: if the simulation cannot verify a safe delayed input, the original timing is retained.
