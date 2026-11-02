# v2.99.6 — Reliability update

- CPU-limited predictions can continue across physics ticks instead of giving up after four resumes.
- New physical inputs no longer cancel a pending prediction. Press/release events are held separately and restored in order after the original input is resolved.
- Robot, UFO, and spider searches now use one full-scoring pass rather than spending half of the time budget on an approximate quick pass.
- Low Latency is disabled on first load because it caps delay at four ticks and prediction time at 3 ms. No-hitch searches are enabled on first load.
- The diagnostic ring buffer retains up to 1,600 events and records input decisions, budget yields, resumes, deferred events, and direct fallbacks.
- Export through the in-level **Copy log** button or the configured export key (F8 by default). The file is saved as `console-export.txt` in the mod's Geode save folder.

## Important limitation

A raw/direct input can still be correct or unavoidable: the prediction may determine that pressing now is already safe, there may be no verified safe delay, or the current situation may be unsupported (for example, platformer mode or ship buffering turned off). This update specifically prevents CPU-budget expiration and later presses/releases from silently cancelling an unfinished search. It does not invent a safe timing when the simulation cannot find one.
