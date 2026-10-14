# HardMob

A Paper, Spigot, and Bukkit plugin for Minecraft 1.21 - 26.3 that adds RPG-style mob leveling and elite bosses. Mobs scale with world age, gain attributes, and can spawn as bosses with custom loot.

[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-green)]()
[![Platform](https://img.shields.io/badge/platform-Paper%20%7C%20Spigot%20%7C%20Bukkit-lightgrey)]()
[![Minecraft](https://img.shields.io/badge/Minecraft-1.21--26.3-green)]()

## What is it

HardMob turns ordinary mobs into a progression system tied to the age of the world. Each mob receives a level from 1 to 99, calculated from the number of in-game days. Higher levels grant more health, damage, speed, knockback resistance, equipment, and, at high tiers, potion effects. A configurable chance turns a spawning mob into an elite boss with multiplied stats, larger size, full armor, and custom drops.

The plugin is built for survival servers, RPG servers, and long-running worlds where vanilla mobs stop being a threat after the first few weeks. Instead of adding new mob types, HardMob modifies existing ones, so it stays compatible with resource packs, mob plugins, and vanilla mechanics. World blacklists and mob whitelists let you keep specific dimensions or entity types untouched.

Level distribution is configurable. You can pick a uniform spread, weight toward low levels, or weight toward high levels. The same applies to equipment thresholds, potion effect chances, and boss stats. All values live in `config.yml`, and most changes apply after `/hardmob reload`.

## Features

- Scales mob levels from 1 to 99 based on in-game days, using a configurable exponential formula.
- Multiplies health, damage, speed, and knockback resistance per level through percentage-based modifiers.
- Assigns armor tiers at level thresholds, with optional enchantment multiplier.
- Applies random potion effects to high-level mobs, with configurable chance, duration, and amplifier.
- Turns a configurable percentage of spawning mobs into elite bosses with multiplied health, damage, speed, and size.
- Drops custom loot from elites, including diamonds, gold, music discs, and bonus experience per level.
- Renders custom nameplates with health, max health, name, level, and boss tag, color-coded by tier.
- Adjusts visual mob size dynamically within a configurable range.
- Supports world blacklist and mob whitelist for precise control over where the plugin runs.
- Provides commands to spawn leveled mobs and bosses manually, with permission nodes for each action.
- Reloads configuration without a server restart through `/hardmob reload`.

## Installation

1. Download the HardMob `1.0.0` JAR.
2. Place the JAR in your server's `plugins` folder.
3. Restart the server. A full restart is preferred over `/reload`.
4. Edit `plugins/HardMob/config.yml` to match your server.
5. Apply changes with `/hardmob reload`; no restart is needed after the first load.

No additional dependencies are required.

## Usage

| Command | Permission | Description |
|---|---|---|
| `/hardmob reload` | `hardmob.reload` | Reloads the configuration. |
| `/hardmob spawn <mob> <level> [elite]` | `hardmob.spawn` | Spawns a leveled mob. The optional `elite` flag makes it a boss. |
| `/hardmob spawnboss <mob> <level>` | `hardmob.spawnboss` | Spawns an elite boss with the given level. |

`<mob>` accepts any Bukkit entity type name, for example `ZOMBIE`, `SKELETON`, or `CREEPER`. `<level>` is an integer between 1 and 99. All permissions default to server operators.

## Configuration

The plugin generates `plugins/HardMob/config.yml`. The default file contains the following sections:

```yaml
general:
  enabled: true

level_system:
  enabled: true
  formula:
    A: 99.0
    B: 98.0
    C: 40.0
  distribution: uniform
  scaling:
    health_percent_per_level: 2.0
    damage_percent_per_level: 1.5
    speed_percent_per_level: 0.3
    knockback_resist_per_level: 0.01
  equipment:
    thresholds:
      20: chainmail
      40: iron
      60: diamond
      80: netherite
    enchant_multiplier: 0.2
  potion_effects:
    chance_per_level: 0.5
    duration_multiplier: 1.0
    amplifier_base: 0
    max_duration_seconds: 600

elite_mobs:
  enabled: true
  spawn_chance_percent: 0.5
  health_multiplier: 2.0
  damage_multiplier: 2.0
  speed_multiplier: 0.7
  size: 2.0
  always_equipped: true
  drops:
    diamonds: { min: 0, max: 4 }
    gold: { min: 0, max: 20 }
    music_discs:
      - MUSIC_DISC_13
    exp_per_level: 10

visual:
  dynamic_size:
    enabled: true
    min: 0.8
    max: 1.2
  custom_name:
    enabled: true
    visible: false
    format: "&8[&c%health%&8/&c%max_health%&8] &f%name% &8[&6%level% lvl&8]"
    format_boss: "&8[&c%health%&8/&c%max_health%&8] &f%name% &8[&4BOSS&8]"
    colors:
      low_level: "GREEN"
      medium_level: "YELLOW"
      high_level: "RED"
      boss: "DARK_RED"

world_blacklist:
  - world_the_end
mob_whitelist: []
debug_mode: false
```

`general.enabled` is the master switch for the whole plugin. `level_system.formula` uses an exponential curve defined by `A`, `B`, and `C`, where `A` is the maximum level after infinite days, `B` is the difference at day zero, and `C` is the number of days until roughly 63 percent of the maximum is reached. `level_system.distribution` accepts `uniform`, `weighted_low`, or `weighted_high`. `level_system.scaling` defines per-level percentage growth for health, damage, speed, and knockback resistance. `level_system.equipment.thresholds` maps level values to armor tiers.

`elite_mobs.spawn_chance_percent` sets the chance for a spawning mob to become a boss. `elite_mobs.drops` defines the loot table and bonus experience. `visual.custom_name` controls the nameplate format, visibility, and color tiers. `world_blacklist` lists worlds where the plugin does not run. `mob_whitelist` restricts which mob types are affected; an empty list means all mobs are allowed. `debug_mode` prints additional information to the console.

## Compatibility

- Minecraft: 1.21 - 26.3
- Platform: Paper, Spigot, Bukkit
- Java: any version supported by the server platform; no specific Java version is required

No known incompatibilities with other plugins or libraries.

## Technologies

- Language: Java
- Platform API: Bukkit, Spigot, Paper
- Build: standard plugin JAR
- Key dependencies: none

## License

This project is distributed under the [GNU General Public License v3.0](LICENSE).