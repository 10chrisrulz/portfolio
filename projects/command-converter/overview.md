# Project Overview

## What It Does

The Minecraft World Converter is a Python-based tool that converts Minecraft 1.12 commands, block data, and world structure into 1.21.10-compatible format. It was built to migrate custom maps, adventure worlds, and command-block systems across 9+ years of Minecraft engine changes.

## Scope

- **Command conversion:** Converts ~40+ command types (e.g., `execute`, `summon`, `fill`, `setblock`, `give`)
- **NBT transformation:** Recursively converts entity, block, and item NBT to 1.21 component format
- **Selector translation:** Maps old selectors (`@e[r=100,c=1]`) to modern syntax (`@e[distance=..100,limit=1,sort=nearest]`)
- **Block & entity mapping:** Uses CSV lookup tables to convert block IDs, data values, and entity names
- **World processing:** Extracts commands from world files, converts them, and reimports results

## Why It Exists

Minecraft 1.12 (2017) and 1.21 (2024+) use different:

- Command syntax (`execute`, `testfor`, `entitydata`, etc.)
- NBT structures (tags vs. components)
- Block and entity namespacing
- Selector parameters and formats

Manual migration of command-heavy worlds is error-prone. This tool automates that migration while keeping logic consistent and traceable.
