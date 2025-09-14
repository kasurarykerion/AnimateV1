# AnimateV1

# Documentation
https://animatedoc-h70zh7df2-kasura0xs-projects.vercel.app/

Powered by [Rojo](https://github.com/rojo-rbx/rojo) 7.5.1.

## Getting Started
To build the place from scratch, use:

```bash
rojo build -o "rojox.rbxlx"
```

Next, open `rojox.rbxlx` in Roblox Studio and start the Rojo server:

```bash
rojo serve
```

For more help, check out [the Rojo documentation](https://rojo.space/docs).

## AnimateV1 (Custom TweenService Replacement)

Path: `src/shared/Modules/AnimateV1.luau`

Features:
- Frame-accurate tweens on `Heartbeat`
- Easing functions: linear, quad/cubic in/out/inOut, backOut, elasticOut, bounceOut
- Delay, repeat, yoyo, timeScale, pause/resume/cancel
- Sequence and parallel combinators
- Works with numbers, Vector2/3, Color3, UDim/UDim2, CFrame, and plain tables
- Bezier path helper `pathQuad` for smooth arcs with optional facing

### Quick Start
```lua
local AnimateV1 = require(game.ReplicatedStorage.Modules.AnimateV1)
local part = workspace.Part

-- Basic property tween (like TweenService:Create + :Play())
local tw = AnimateV1.to(part, { time = 1 }, { Transparency = 1, Color = Color3.new(1,0,0) })
tw:play()
tw:await() -- wait for completion

-- Yoyo repeat
local tw2 = AnimateV1.to(part, { time = 0.6, repeatCount = 3, yoyo = true }, { Size = Vector3.new(6,6,6) })
tw2:play()

-- Pause/resume
task.wait(0.2)
tw2:pause()
task.wait(0.2)
tw2:resume()
  
-- Sequence and parallel
local s = AnimateV1.sequence({
  AnimateV1.to(part, { time = 0.5 }, { Transparency = 0.5 }),
  0.2,
  function() print("midpoint") end,
  AnimateV1.parallel({
    AnimateV1.to(part, { time = 0.6 }, { Color = Color3.new(0,1,0) }),
    AnimateV1.to(part, { time = 0.6 }, { Reflectance = 0.2 }),
  })
})
s:play()
  
-- Bezier arc path
local p0 = workspace.P1.Position
local p2 = workspace.P2.Position
local c = (p0 + p2)/2 + Vector3.new(0, 25, 0)
local arc = AnimateV1.pathQuad(workspace.P3, { time = 0.8, faceDirection = true }, p0, c, p2)
arc:play()
arc:await()
```

Notes:
- For table targets (non-Instances), you can tween arbitrary numeric fields, e.g., `{x=0,y=0}`.
- `timeScale` lets you speed up/slow down a tween on the fly with `tw:setTimeScale(0.5)`.
- All operations are time-based (delta-time integrated), not frame-count-based.
