# Use with Rojo

This guide shows how to include `AnimateV1` in your Roblox project using Rojo.

## Folder layout

Recommended structure in your repo:

```
repo/
├─ default.project.json
├─ src/
│  ├─ shared/
│  │  └─ Modules/
│  │     └─ AnimateV1.luau   <-- the module
│  └─ client/
│  └─ server/
└─ docs/
```

Ensure your `default.project.json` maps `src` into the game tree (e.g., `ReplicatedStorage/Shared`). Example snippet:

```json
{
  "$schema": "https://rojo.space/schema/project.json",
  "name": "AnimateV1Project",
  "tree": {
    "$className": "DataModel",
    "ReplicatedStorage": {
      "$className": "ReplicatedStorage",
      "Shared": {
        "$className": "Folder",
        "Modules": {
          "$path": "src/shared/Modules"
        }
      }
    }
  }
}
```

> Note: If your IDE warns about a missing schema, you can remove `$schema` or install the Rojo VSCode extension providing the schema.

## Using the module in code

After syncing with Rojo (`rojo serve` and Roblox Studio plugin – or `rojo build`):

```lua
local AnimateV1 = require(game.ReplicatedStorage.Shared.Modules.AnimateV1)

-- Example usage
local tw = AnimateV1.to(workspace.Part, { time = 0.6, easing = AnimateV1.Easing.cubicInOut }, {
  Transparency = 0.5,
})
tw:play()
```

## Tips

- Keep `AnimateV1.luau` under `src/shared/Modules` so both client and server can require it.
- If you rename folders, update both the Rojo mapping and your `require` paths.
- For packages, you can also use Wally to manage dependencies and place the module under a `Packages/` folder mapped by Rojo.
