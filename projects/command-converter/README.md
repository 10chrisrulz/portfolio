# Minecraft 1.12 → 1.21.10 Command & World Converter

Portfolio documentation for a **Python** tool that migrates Minecraft worlds and command blocks across major version boundaries (1.12 → 1.21.10).

---

## What It Does

The converter transforms Minecraft 1.12 commands, block data, and world structure into 1.21.10-compatible format. It was built to migrate custom maps, adventure worlds, and command-block systems across 9+ years of engine changes.

---

## Scope

- **Command conversion:** ~40+ command types (e.g., `execute`, `summon`, `fill`, `setblock`, `give`)
- **NBT transformation:** Recursively converts entity, block, and item NBT to 1.21 component format
- **Selector translation:** Maps old selectors (`@e[r=100,c=1]`) to modern syntax (`@e[distance=..100,limit=1,sort=nearest]`)
- **Block & entity mapping:** CSV lookup tables for block IDs, data values, and entity names
- **World processing:** Extracts commands from world files, converts them, and reimports results

---

## Why It Exists

Minecraft 1.12 (2017) and 1.21 (2024+) differ in:

- Command syntax (`execute`, `testfor`, `entitydata`, etc.)
- NBT structures (tags vs. components)
- Block and entity namespacing
- Selector parameters and formats

Manual migration of command-heavy worlds is error-prone. This tool automates it while keeping logic consistent and traceable.

---

## Docs in This Folder

| Document | Description |
|----------|-------------|
| [README.md](README.md) | This overview. |
| [architecture.md](architecture.md) | Tech stack, structure, and design decisions. |
| [features.md](features.md) | Command coverage and conversion types. |
| [challenges.md](challenges.md) | Technical challenges and solutions. |

---

## Quick Reference

- **Primary script:** `command_converter.py` (~7.5k lines)
- **Supporting:** `convert_single_world.py`, `batch_convert_worlds.py`, `extract_commands.py`, `reimport_commands_simple.py`
- **Reference:** mcstacker-1.21.10.min.js for target format validation
