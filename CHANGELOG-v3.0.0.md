# v3.0.0 — FPS-neutral physics scheduling and deeper wave lookahead

- Replaced the 960-TPS gameplay override with a fixed standard 240-TPS clock. This preserves the conventional GD physics cadence while normalizing it across 30/60/high render FPS.
- Removed the 3 ms wall-clock gate between no-hitch search continuations. A pending search now receives one 0.75 ms slice per physics update.
- Capped the initial prediction slice too, so a click does not perform a large one-frame burst before yielding.
- Removed render-interval-based auto-budget scaling from the decision path.
- Raised the effective wave horizon clamp from 160 to 240 ticks, matching the settings UI.
- Kept the existing wave full-horizon scoring, adjacent-timing margin and robust-position checks.

## Important

The attached diagnostic export is from v2.99.7, not v2.99.9. It shows 240 physics steps/s, a 4 ms configured/effective budget, 20 budget yields/resumes, and one `UNBUFFERED` decision after the search had only reached +37 ticks of a 128-tick window. That is consistent with a search-work scheduling limit, not proof that the wave physics model itself is wrong.

This source has not been compiled against the Geode SDK in this environment. Build with the repository's GitHub Actions workflow before installation.
