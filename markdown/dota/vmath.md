# VMath
Vector math functions

### Examples

Getting the direction vector from the caster to the point when using DOTA_ABILITY_BEHAVIOR_POINT

```lua
require("VMath")

function my_ability:OnSpellStart()
  local direction = VMath:GetCursorDirection(self)
end
```

Rotates the caster around the world zero point by 90 degrees

```lua
local nextPosition = VMath:RotateAround(
  caster:GetAbsOrigin(),
  Vector(0,0,0)
  90
)
caster:SetAbsOrigin(nextPosition)
```

---

### VMath.lua
```lua
if VMath == nil then
  _G.VMath = class({})
end

--returns a normalized direction vector from caster position to cursor position
function VMath:GetCursorDirection(ability)
  local caster = ability:GetCaster()
  local custorPos = ability:GetCursorPosition()
  local direction = custorPos - caster:GetAbsOrigin()
  direction.z = 0
  direction = direction:Normalized()
  return direction
end

--rotate a world space point around another an origin point in world space on the z-axis by a given angle (degrees)
function VMath:RotateAround(point, origin, angle)
  local rAngle = math.rad(angle)

  local pos = point - origin
  local x = (pos.x * math.cos(rAngle)) - (pos.y * math.sin(rAngle))
  local y = (pos.x * math.sin(rAngle)) + (pos.y * math.cos(rAngle))

  local result = Vector(
    x + origin.x,
    y + origin.y,
    point.z
  )

  return result
end
```

