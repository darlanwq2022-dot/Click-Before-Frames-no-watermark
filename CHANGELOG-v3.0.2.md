# v3.0.2 — Build fix

- Fixed the two Clang errors reported by Windows and Android builds: `alignOrb()` referenced `searchFrom` before its declaration. The active lower-bound value is now passed explicitly from `intercept()`. This preserves the intended no-hitch resume behavior and Tick Tuning minimum bound.
- Marked `BufferMenu::init()` as `override`, removing the Clang `-Winconsistent-missing-override` warning.
- No prediction algorithm, timing, tick limits, or gameplay settings were changed in this patch.
