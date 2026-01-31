# Data Sources and Flow

High-level map of where data comes from and where it goes in the MineZ data scraping project.

---

## Inputs

### Primary

- **player_data.db**  
  SQLite database of player save states (e.g. exported from game/server backups). Contains at least player identifier (UUID) and a status/inventory payload (often JSON in a column). Scripts assume a stable schema (e.g. one row per save or per player with “latest” semantics).

- **opt-ins.csv**  
  CSV of player UUIDs who have opted in to having their item ownership shown on the public site. Read with UTF-8 (with BOM handling where needed). No other columns required for the ownership pipeline.

### Supporting

- **Bukkit_1.12_Lookups.csv**  
  Lookup table (e.g. `raw_block`, `data`, `conversion name`) used to map material + data values to human-readable item names. Used when deriving item type/name for things like potions or skulls so that deterministic item UUIDs match the rest of the ecosystem.

- **items.json** (from item tracking)  
  Produced by other scripts (e.g. `item_tracking_new_methodology.py`). Contains item metadata and metrics (e.g. “currently active”). The ownership pipeline does not create this file but the same item UUID logic is used so that ownership DB and items.json refer to the same items.

---

## Outputs

| Output | Produced By | Purpose |
|--------|-------------|---------|
| **item-ownership.db** | `generate_item_ownership.py` | Canonical ownership + opt-in data for the website. |
| **items.json** | Item tracking scripts | Item list and metrics for the site. |
| **CSV/JSON exports** | Various scripts | One-off analysis, imports, or backups. |

---

## Data Flow (Simplified)

1. **Raw data** — Player saves live in `player_data.db`; opt-in list in `ownership/opt-ins.csv`.
2. **Item tracking** — Separate pipeline(s) read player_data (or similar) and produce `items.json` and any CSV/metrics using the same deterministic item UUID logic.
3. **Ownership pipeline** — `generate_item_ownership.py` reads `player_data.db` and `opt-ins.csv`, optionally uses Bukkit lookups, and writes `ownership/item-ownership.db`.
4. **Website / API** — Read `item-ownership.db` (and `items.json`) to serve “who has this item” and item lists; filter ownership by `player_opt_in` so only opted-in players are shown.

---

## Consistency Notes

- **Item UUIDs** must be computed the same way in:
  - `generate_item_ownership.py`
  - `item_tracking_new_methodology.py`
  - Any other script that emits or consumes item IDs.  
  That way, an item in `items.json` and an item in `item-ownership.db` share one UUID.

- **Opt-in** is the single source of truth for “show this player’s ownership”; the ownership DB stores full ownership and the opt-in table is used at read time to filter.

This doc is intentionally high-level; file paths and table names may vary slightly—see the repo and `ownership/` docs for exact schemas and paths.
