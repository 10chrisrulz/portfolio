# MineZ Data Scraping — Project Overview

A Python-based data pipeline for the **MineZ** community that ingests player save data, derives item tracking metrics, and produces structured outputs (SQLite, JSON, CSV) for the [MineZ Item Flow Tracker](../../portfolio/README.md) website. The work focuses on reliability, deterministic identifiers, and opt-in–aware ownership data.

---

## What This Pipeline Does

- **Ingests** SQLite player save snapshots (`player_data.db`) and optional lookup tables (e.g. Bukkit material mappings).
- **Extracts** item metadata (type, name, lore, enchantments, damage, skull texture) and normalizes it for consistent identification.
- **Generates** deterministic item UUIDs so the same logical item is represented by the same ID across scripts and the website.
- **Builds** an **item ownership** database (`item-ownership.db`) that records which players own which items, respecting an opt-in list (`opt-ins.csv`).
- **Outputs** metrics and artifacts (e.g. `items.json`, CSV exports) consumed by the frontend and APIs.

---

## High-Level Architecture

```
player_data.db  +  opt-ins.csv  +  lookup tables
         │
         ▼
  generate_item_ownership.py  (and related scripts)
         │
         ├──► item-ownership.db   (ownership + opt-in)
         ├──► items.json          (from item_tracking_*)
         └──► other exports (CSV, etc.)
```

---

## Key Concepts

- **Deterministic item UUIDs**: Items are identified by a hash of (type, name, lore, enchantments, damage when applicable, skull texture). This keeps the same “logical” item (e.g. a specific named sword) stable across runs and systems.
- **Opt-in ownership**: Only players listed in `opt-ins.csv` are treated as opted in; the ownership DB stores full ownership data but the website uses the opt-in set to decide what to show.
- **Compressed ownership storage**: Ownership lists per item are stored as gzip-compressed JSON in SQLite for space and I/O efficiency.

---

## Tech Stack

| Layer | Technology |
|-------|-------------|
| Language | Python 3 |
| Storage | SQLite (player data and item-ownership DBs) |
| Config / Export | CSV (opt-ins, lookups), JSON |
| Compression | gzip (blobs in ownership DB) |

---

## Docs in This Folder

| Document | Purpose |
|----------|---------|
| [README.md](README.md) | This overview. |
| [DATA_SOURCES.md](DATA_SOURCES.md) | Inputs, outputs, and data flow. |
| [ITEM_OWNERSHIP.md](ITEM_OWNERSHIP.md) | Item ownership pipeline, schema, and usage. |

---

Suitable for portfolio use: high-level only; implementation details live in the codebase and in repo-specific docs (e.g. schema, storage comparisons).
