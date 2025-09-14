# API

## Easing

- `AnimateV1.Easing.linear(t)` and many more: `quadIn/Out/InOut`, `cubicIn/Out/InOut`, `sineIn/Out/InOut`, `expoIn/Out/InOut`, `circIn/Out/InOut`, `backIn/Out/InOut`, `elasticIn/Out/InOut`, `bounceIn/Out/InOut`
- `Easing.fromCurve(points: {Vector2}) -> (t)->number`

## Core Tweens

```lua
local t = AnimateV1.to(target, {
  time = 1.0,
  easing = AnimateV1.Easing.cubicInOut,
  delay = 0,
  repeatCount = 0, -- -1 infinite
  yoyo = false,
  timeScale = 1,
}, {
  Transparency = 0.5,
  Color = Color3.fromRGB(255,120,80),
})

-- Control
t:play()
t:pause()
t:resume()
t:cancel()
t:await()
t:setTimeScale(0.5)
```

- `AnimateV1.from(target, info, fromValues, toValues)` sets initial values first then tweens to `toValues`.
- Supports: number, Vector2, Vector3, Color3, UDim, UDim2, CFrame, and arbitrary table fields.

## Composition

- `AnimateV1.sequence({ step1, 0.2, step2, function() ... end })`
- `AnimateV1.parallel({ tweenA, tweenB })`

## Paths

- `pathQuad(part, info, p0, c, p2)`
- `pathQuadConstSpeed(part, info, p0, c, p2)`
- `pathHermite(part, info, p0, m0, p1, m1)`
- `pathCatmullRom(part, info, {points})`

Common `info` fields: `{ time, easing?, faceDirection?, bank? }`

## Keyframes

```lua
local tracks = {
  Transparency = {
    { t = 0.0, value = 0 },
    { t = 0.5, value = 1, easing = AnimateV1.Easing.sineInOut },
    { t = 1.0, value = 0 },
  },
  Color = {
    { t = 0.0, value = Color3.fromRGB(255,255,255) },
    { t = 1.0, value = Color3.fromRGB(0,255,0) },
  }
}
AnimateV1.keyframes(workspace.P3, 2.0, tracks):play()
```

## Springs

- `spring(target, params, goals)` with params `{ stiffness?, damping?, mass?, velocity? }` for `number` and `Vector3` properties.

## Timeline

```lua
local TL = AnimateV1.Timeline.new({ loopMode = "pingpong", repeatCount = -1, timeScale = 1 })
TL:addTween(AnimateV1.to(part, { time = 0.5 }, { Transparency = 0.5 }), 0.0)
TL:addLabel("half")
TL:addTween(AnimateV1.to(part, { time = 0.5 }, { Transparency = 0 }), 0.6)
TL:play()
-- TL:seekLabel("half")
-- TL:pause(); TL:resume()
```
