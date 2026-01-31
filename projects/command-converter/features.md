# Key Features

## Command Coverage

| Category | Examples |
|----------|----------|
| **Flow** | `execute`, `testfor`, `testforblock`, `testforblocks` |
| **World** | `setblock`, `fill`, `clone`, `blockdata` |
| **Entities** | `summon`, `entitydata`, `tp`, `kill` |
| **Items** | `give`, `clear`, `replaceitem` |
| **Players** | `title`, `tellraw`, `tell`, `effect`, `scoreboard` |
| **Media** | `playsound`, `stopsound`, `particle` |
| **Custom** | `project`, `script`, `clock` (Dungeons-style) |

---

## Conversion Types

- **Blocks:** ID + data value → modern block name with block states
- **Entities:** Old names → `minecraft:` namespaced IDs
- **Selectors:** `r=` → `distance=..`, `c=` → `limit=` + `sort=nearest`, `m=` → `gamemode=`
- **Items:** NBT tags → components (`minecraft:custom_name`, `minecraft:lore`, `minecraft:profile`, etc.)
- **Sounds:** Legacy names → 1.21 sound IDs
- **Particles:** Old particle names and parameters → 1.21 format

---

## Output & Validation

- **Failure logging:** Tracks unknown commands, failed block/entity lookups, and NBT issues
- **Batch mode:** Processes multiple worlds with progress reporting
- **Integration:** Can be used as a library (`CommandConverter.convert_command()`) or via CLI scripts
