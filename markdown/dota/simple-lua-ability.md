# Simple Lua Ability
1 - First make a lua script to do damage

### vscripts/my\_spell\_name.lua
```lua
my_spell_name = class({})

--event when ability completes cast point
function my_spell_name:OnSpellStart()
  local caster = self:GetCaster() --unit casting ability
  local target = self:GetCursorTarget() --unit clicked on
  if target ~= nil then
    --apply damage
    ApplyDamage({
      victim = target,
      attacker = caster,
      damage = self:GetAbilityDamage(), --get data from kv file
      damage_type = self:GetAbilityDamageType(),
      ability = self
    })
  end
end
```

---

2 - Next add it to the abilities key value file

### npc/npc\_abilities\_custom.txt
```
"DOTAAbilities"
{
  "Version"		"1"

  "my_spell_name"
  {
    "BaseClass" "ability_lua"
    "ScriptFile" "my_spell_name"

    "AbilityBehavior"       "DOTA_ABILITY_BEHAVIOR_UNIT_TARGET"
    "AbilityTextureName"    "bounty_hunter_shuriken_toss"
    "AbilityDamage"         "200"
    "AbilityUnitDamageType" "DAMAGE_TYPE_PURE"
    "AbilityManaCost"       "0"
    "AbilityCooldown"       "4.0"
  }
}
```

---

3 - Then add the ability in the heroes key value file. This will override a hero\'s default ability loadout

### npc/npc\_heroes\_custom.txt
```
"DOTAHeroes"
{
  //overriding hero wraith king
  "wraith_king"
  {
		"override_hero"   "npc_dota_hero_skeleton_king"
		"Ability1"        "my_spell_name"
  }
}

```
