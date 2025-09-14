# Features

Below is a tour of the key features of AnimateV1 with short, copy‑paste code snippets. All examples assume:

```lua
local AnimateV1 = require(game.ReplicatedStorage.Modules.AnimateV1)
local Easing = AnimateV1.Easing
```

## Basics

```lua
-- Tween multiple properties
AnimateV1.to(workspace.Part, { time = 0.6, easing = Easing.cubicInOut }, {
  Transparency = 0.25,
  Color = Color3.fromRGB(255, 120, 80),
}):play()

-- Delay, repeat and yoyo
AnimateV1.to(workspace.Part, { time = 0.4, delay = 0.2, repeatCount = 2, yoyo = true }, {
  Size = Vector3.new(6,6,6),
}):play()
```

## Easing Pack

```lua
-- Use any easing function
AnimateV1.to(workspace.Part, { time = 0.8, easing = Easing.bounceOut }, { Transparency = 0.5 }):play()

-- Custom curve using points in [0,1]
local curve = Easing.fromCurve({ Vector2.new(0,0), Vector2.new(0.2,0.1), Vector2.new(0.8,1.0), Vector2.new(1,1) })
AnimateV1.to(workspace.Part, { time = 0.8, easing = curve }, { Reflectance = 0.2 }):play()
```

## Sequences

```lua
local seq = AnimateV1.sequence({
  AnimateV1.to(workspace.Part, { time = 0.2 }, { Transparency = 0.3 }),
  0.1,
  function() print("Halfway!") end,
  AnimateV1.to(workspace.Part, { time = 0.3 }, { Transparency = 0 }),
})
seq:play()
```

## Parallel

```lua
AnimateV1.parallel({
  AnimateV1.to(workspace.Part, { time = 0.6 }, { Color = Color3.fromRGB(0,255,0) }),
  AnimateV1.to(workspace.Part, { time = 0.6 }, { Reflectance = 0.2 }),
}):play()
```

## Paths: Bezier (Quadratic)

```lua
local p0 = workspace.P1.Position
local p2 = workspace.P2.Position
local c  = (p0 + p2)/2 + Vector3.new(0, 25, 0)

-- Simple arc
AnimateV1.pathQuad(workspace.Part, { time = 0.8, easing = Easing.sineInOut }, p0, c, p2):play()

-- Near-constant speed motion along the arc (uses arc-length LUT)
AnimateV1.pathQuadConstSpeed(workspace.Part, { time = 0.8, faceDirection = true }, p0, c, p2):play()
```

## Paths: Hermite (Tangents) with Banking

```lua
local m0 = Vector3.new(20, 40, 0) -- tangent from p0
local m1 = Vector3.new(20, -20, 0) -- tangent into p1
AnimateV1.pathHermite(workspace.Part, {
  time = 1.2,
  faceDirection = true,
  bank = math.rad(10),
}, workspace.P1.Position, m0, workspace.P2.Position, m1):play()
```

## Paths: Catmull‑Rom Chain with Facing + Banking

```lua
local pts = {
  Vector3.new(-30,0,0),
  Vector3.new(-10,10,10),
  Vector3.new(10,15,-5),
  Vector3.new(30,0,0),
}
AnimateV1.pathCatmullRom(workspace.Part, {
  time = 2.0,
  faceDirection = true,
  bank = math.rad(15),
}, pts):play()
```

## Keyframes (Per-Property Easing)

```lua
local tracks = {
  Transparency = {
    { t = 0.00, value = 0 },
    { t = 0.50, value = 1, easing = Easing.sineInOut },
    { t = 1.00, value = 0 },
  },
  Color = {
    { t = 0.00, value = Color3.fromRGB(255,255,255) },
    { t = 1.00, value = Color3.fromRGB(0,255,0) },
  }
}
AnimateV1.keyframes(workspace.Part, 2.0, tracks):play()
```

## Spring Tweens (Number/Vector3)

```lua
-- Natural motion to a Vector3 goal
AnimateV1.spring(workspace.Part, { stiffness = 220, damping = 26 }, {
  Position = workspace.P2.Position,
}):play()
```

## Timeline (Seekable, Looping, Labels)

```lua
local TL = AnimateV1.Timeline.new({ loopMode = "pingpong", repeatCount = -1, timeScale = 1 })
TL:addTween(AnimateV1.to(workspace.Part, { time = 0.5 }, { Transparency = 0.5 }), 0.0)
TL:addLabel("half")
TL:addTween(AnimateV1.to(workspace.Part, { time = 0.5 }, { Transparency = 0.0 }), 0.6)

TL:play()
-- TL:seek(0.3)
-- TL:seekLabel("half")
-- TL:pause(); TL:resume()
```

## Instant Scrubbing for Debugging

```lua
local tw = AnimateV1.to(workspace.Part, { time = 1.0 }, { Transparency = 1 })
-- Jump instantly to any point without playing
tw:setProgress(0.75)
```

## Supported Value Types

- number, Vector2, Vector3, Color3, UDim, UDim2, CFrame
- Arbitrary table fields on your own tables

> Tip: If a property type isn’t supported, you can blend it yourself and call `applyProperties` on a custom target table, then apply its fields in your own step.
