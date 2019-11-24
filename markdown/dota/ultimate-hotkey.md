# Ultimate Hotkey Fix
This is how you get the ultimate hotkey to be set for an ability. You must have your ability in slot 6 and have an ability in each slot from 1-5. If you don't have an ability for all the slots then you can add a hidden ability into the empty slots. This way the hidden abilities won't show up in the HUD.

### npc_abilities_custom.txt
```
"hidden_abil"
{
  "BaseClass"        "ability_datadriven"
  "AbilityBehavior"  "DOTA_ABILITY_BEHAVIOR_HIDDEN"
}
```

### npc_heroes_custom.txt
```
"wraith_king"
{
  "override_hero"  "npc_dota_hero_skeleton_king"
  "Ability1"       "hidden_abil"
  "Ability2"       "hidden_abil"
  "Ability3"       "hidden_abil"
  "Ability4"       "hidden_abil"
  "Ability5"       "hidden_abil"
  "Ability6"       "wraith_king_r"
}
```
