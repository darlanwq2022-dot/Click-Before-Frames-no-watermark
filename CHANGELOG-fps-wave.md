# v2.99.9 — FPS-independent physics + wave consistency

- Physics TPS default changed from 240 to 960, with a clear warning that higher TPS changes game physics. Rendering FPS can stay at 30/60/144 while the fixed physics step runs at 960 TPS.
- Reworked the fixed-step accumulator: bounded backlog, no wholesale time reset when a catch-up cap is reached, and a 250 ms per-render-frame catch-up ceiling.
- No-hitch continuations now use a 1 ms slice and a 3 ms wall-time cadence instead of receiving a fresh full search budget on every physics substep. This prevents the 960-TPS loop from multiplying the budget dozens of times inside one rendered frame.
- Resume elapsed time is derived from the original input tick, so throttling does not undercount how long the pending input has waited. Remaining delay bounds and minimum tick are adjusted accordingly.
- Wave search requires full-horizon success for a full save, checks positive and negative position offsets, and uses the center of a three-candidate safe window when one exists. If no full-horizon save exists, it may accept a partial trajectory that improves total time-to-death by at least 12 ticks and passes the same robustness check.
- Wave horizon default raised from 65 to 160 ticks; max raised to 240.
- Auto budget default changed to OFF so the configured search budget is not cut according to render-frame interval.
- Static JSON/source checks pass. A compiled `.geode` is not included because the Geode SDK is not installed in this environment.
