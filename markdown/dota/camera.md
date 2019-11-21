# Camera
Focusing the camera onto the players hero

### Example
```lua
--this will move the camera for all players centered on their hero
Camera:FocusHeroForAllPlayers()
```

---

### Camera.lua
- *dependency for Task.lua, requires you to also have task.lua*

```lua
if Camera == nil then
  _G.Camera = class({})
end

require("Task")
local PLAYERS_PER_TEAM = 5 --change this if you have different number of players

--move all players camera to their hero
function Camera:FocusHeroForAllPlayers()
  for teamID=TEAM_FIRST, DOTA_TEAM_CUSTOM_MAX do
    for i=1, PLAYERS_PER_TEAM do
      local playerID = PlayerResource:GetNthPlayerIDOnTeam(teamID, i)
      if PlayerResource:IsValidPlayerID(playerID) then
        local player = PlayerResource:GetPlayer(playerID)
        if player ~= nil then
          local hero = player:GetAssignedHero()
          if hero ~= nil then
            PlayerResource:SetCameraTarget(playerID, hero)
          end
        end
      end
    end
  end

  Task:Delay(Camera.OnRemoveFocus, 0.08, {})
end

function Camera:OnRemoveFocus()
  for teamID=TEAM_FIRST, DOTA_TEAM_CUSTOM_MAX do
    for i=1, PLAYERS_PER_TEAM do
      local playerID = PlayerResource:GetNthPlayerIDOnTeam(teamID, i)
      if PlayerResource:IsValidPlayerID(playerID) then
        PlayerResource:SetCameraTarget(playerID, nil)
      end
    end
  end
  return -1
end
```