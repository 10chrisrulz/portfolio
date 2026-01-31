# Item Ownership Pipeline

High-level description of how item ownership data is produced and stored for the MineZ item tracker.

---

## Goal

Produce a database (`item-ownership.db`) that answers:

- Which items exist (by deterministic UUID).
- Which players own each item (with quantities).
- Which players have opted in to having their ownership shown on the site.

The website uses this DB plus the opt-in set to display “who has this item” only for opted-in players.

---

## Inputs

| Input | Role |
|-------|------|
| `player_data.db` | SQLite DB of player save states; each row typically has player UUID and a status/inventory payload. |
| `opt-ins.csv` | List of player UUIDs who have opted in to public ownership display. |
| `Bukkit_1.12_Lookups.csv` (optional) | Maps raw material + data values to display names (e.g. for potions, skulls). |

---

## Pipeline (High-Level)

1. **Load opt-ins** — Read `opt-ins.csv` (e.g. UTF-8 with BOM handling) into a set of opted-in UUIDs.
2. **Scan player data** — For each player, take the latest save (or designated snapshot); parse status/inventory JSON.
3. **Extract items** — From each save, extract item type, custom name, lore, enchantments, damage (when relevant), skull texture. Normalize and strip Minecraft formatting where needed.
4. **Item UUID** — Compute a deterministic UUID from those attributes (aligned with `item_tracking_new_methodology.py` so item IDs match across the project).
5. **Aggregate ownership** — Build (item_uuid → list of (player_uuid, quantity)).
6. **Write DB** — Insert into SQLite: opt-in table (UUID presence = opted in) and item_ownership table with compressed ownership blobs.

---

## Output Schema (Conceptual)

- **player_opt_in** — One row per opted-in player (UUID as primary key). Presence = opted in.
- **item_ownership** — One row per distinct item (by item UUID):
  - `item_uuid` (primary key)
  - `ownership_data` — gzip-compressed JSON array of `{ playerUuid, quantity }`
  - `owner_count` — Denormalized count for quick “how many owners” queries
  - Optional metadata (e.g. `data_size`) for monitoring

Indexes on `owner_count` (and any date/snapshot columns, if used) support fast listing and filtering.

---

## Design Choices

- **Deterministic UUIDs**: Same item definition ⇒ same UUID everywhere (scripts, DB, website), so no cross-reference drift.
- **Opt-in in a separate table**: Simple “in table = opted in” model; ownership data itself can cover all players so the backend can filter by opt-in at query time.
- **Compressed blobs**: Reduces DB size and I/O; decompression cost per item is small compared to network and UI work.
- **No username in ownership DB**: Usernames are resolved elsewhere (e.g. API or separate service); keeps the DB focused and avoids staleness.

---

## Usage

- **Generate/refresh**: Run `generate_item_ownership.py` (with correct paths to `player_data.db`, `ownership/opt-ins.csv`, and output `ownership/item-ownership.db`).
- **Query owners**: Use a small helper (e.g. `get_item_owners.py`) or query the DB directly: decompress `ownership_data` for an `item_uuid`, then filter by `player_opt_in` if only opted-in owners should be shown.

For full schema SQL and size/performance notes, see repo-specific docs such as `ownership/OPTIMIZED_DB_SCHEMA.md`.
