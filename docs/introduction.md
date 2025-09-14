# Introduction

`AnimateV1` is a Roblox tweening library designed to exceed `TweenService` limitations.

- Frame-accurate, time-based updates using `RunService.Heartbeat`.
- Consistent easing and powerful composition (`sequence`, `parallel`).
- Advanced motion: Bezier, Hermite, Catmull‑Rom, constant-speed arcs.
- Keyframe animation with per-key easings.
- Spring dynamics for natural motion.
- Seekable `Timeline` with loop modes: `clamp`, `pingpong`, `mirror`.

## Why not TweenService?

TweenService is convenient but limited. AnimateV1 adds:

- Custom easing functions and curves.
- Arbitrary table tweening (not only Instances).
- Real paths and path-facing with optional banking.
- Deterministic scrubbing via `setProgress()`.
