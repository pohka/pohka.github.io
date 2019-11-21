# Query Units
Custom query functions to simplify searching within custom games



### Example
```lua
require("Query")

function MyAbility:OnSpellStart()
  local caster = self:GetCaster()

  --find an item by name on the caster
  local item = Query:FindItemByName(caster, "item_blink")
  if item ~= nil then
    ...
  end

  --find all units in sector in front of caster
  --total angle = 180 degrees
  local radius = 500
  local angle = 90
  local units = Query:FindUnitsSector(
    caster:GetTeam(),
    caster:GetAbsOrigin(),
    caster:GetForwardVector(),
    angle,
    nil,
    radius,
    DOTA_UNIT_TARGET_TEAM_ENEMY,
    DOTA_UNIT_TARGET_HERO,
    DOTA_UNIT_TARGET_FLAG_NONE,
    FIND_ANY_ORDER,
    false,
    nil
  )
  for _,unit in pairs(units) do
    print("found unit:", unit:GetName())
  end
end
```
---

### Query.lua
```lua
if Query == nil then
  Query = class({})
end

--find an item on the given unit with a matching name, returns nil if not found
function Query:FindItemByName(unit, itemName)
  if unit ~= nil and unit:HasInventory() then
    for i=DOTA_ITEM_SLOT_1, DOTA_STASH_SLOT_6 do
      local item = unit:GetItemInSlot(i);
      if item:GetName() == itemName then
        return item
      end
    end
  end
  return nil
end

--find an ability on the given unit with a matching name, returns nil if not found
function Query:FindAbilityByName(unit, abilityName)
  local count = unit:GetAbilityCount() - 1
  for i=0, count do
    local abil = unit:GetAbilityByIndex(i)
    if abil ~= nil then
      if abil:GetName() == abilityName then
        return abil
      end
    end
  end
  return nil
end

--find units in sector shape (e.g. 90 degrees is a semi circle)
--angle in degrees from 0-180
function Query:FindUnitsSector(
  teamNumber,
  position,
  forward,
  angle,
  cacheUnit,
  radius,
  teamFilter,
  typeFilter,
  flagFilter,
  order,
  cacheCanGrow
  )
  local units = FindUnitsInRadius(
    teamNumber,
    position,
    cacheUnit,
    radius,
    teamFilter,
    typeFilter,
    flagFilter,
    order,
    cacheCanGrow
  )

  local result = {}
  for _,unit in pairs(units) do
    local diff = unit:GetAbsOrigin() - position
    diff.z = 0
    local dir = diff:Normalized()
    local dot = dir:Dot(forward)
    --arccos and then converting from radians to degrees
    local angleDiff = math.deg(math.acos(dot)) 
    if angleDiff <= angle then
      table.insert(result, unit)
    end
  end

  return result
end
```
