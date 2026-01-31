# Implemented Features (Full Scope)

This document lists only features that are **currently implemented**. Map-related functionality is out of scope.

---

## Search & Filters

- **Multi-field search**: Up to three search inputs; matches against item name, lore, enchantments (and translated enchantment names). Debounced (300 ms).
- **Quote normalization**: Typographic apostrophes and quotes (e.g. from mobile keyboards) are normalized so searches like “agni's” match correctly.
- **Hide Heal Kit**: Filter out heal kit items.
- **Hide Component Bag**: Filter out component bag items.
- **Hide Player Heads**: Filter to exclude player head variants.
- **Group by name**: Aggregate items that share the same cleaned name + type (and skull texture where applicable); share links disabled for grouped rows.
- **Currently exists**: Filter to items with `CurrentlyActive > 0`.

---

## Sorting

- Sort options: **Name**, **Usage rate**, **Yearly created**, **Total seen**, **Currently active**, **Total removed** (direction depends on option).

---

## Item List & Cards

- **Infinite scroll**: Initial page load (e.g. 20 items), then load more on scroll.
- **Item cards**: Color-coded Minecraft names, lore rendering, skull textures (mc-heads.net) for skull items.
- **Share link**: Per-item share URL (disabled when “group by name” is on).
- **Responsive layout**: Grid adapts to screen size (e.g. 3 → 2 columns).

---

## Item Detail View

- **Expanded stats**: Total created, currently active, total removed, usage rate, yearly created/removed (e.g. 2024/2025).
- **Lore**: Rendered with Minecraft-style formatting.
- **Skull texture**: Shown for skull items (player or static).
- **Monthly chart**: Created/removed by month with year selector; wraps below metrics on small screens.
- **Metrics layout**: Responsive grid (e.g. 3 columns → 2 columns on narrow screens), with “Removed (year)” and “Created (year)” on the bottom row when wrapped.
- **Actions**: Copy (share URL), Readme (metrics docs), Owners (opens ownership modal when opted-in owners exist).

---

## Item Ownership (Opt-in)

- **Owners button**: Shown on item detail; enabled only if at least one opted-in owner exists (lightweight API check on load).
- **Owners modal**: Sheet-style overlay listing owners with:
  - Player skull (mc-heads.net)
  - Username (resolved from UUID via proxy API, with client-side caching)
  - Quantity owned
- **Sorting**: Owners sorted by quantity (highest first).
- **Pagination**: Initial load (e.g. 15); support for loading more (infinite scroll).
- **Description**: “X of Y owners opted in for public data sharing” (Y = total owners; X = opted-in count). Info tooltip explains opt-in and points to changelog.
- **Privacy**: Only players present in the opt-in table are returned; non-opted-in data is never sent to the client.

---

## Charts & Visuals

- **Monthly chart**: Line/area for created and removed per month; year selector; responsive.
- **Metric tooltips**: In-detail view where applicable.

---

## Sharing & Meta

- **OG images**: Dynamic Open Graph image per item (`/api/og/[uuid]`) for link previews (name, key metrics, chart).
- **Deep links**: Item detail reachable via `?item=<uuid>` and `/item/[uuid]`.

---

## Header & Docs

- **Header**: Title, subheader (“Track item statistics, usage rates, and flow data”), Changelog and Readme buttons, “Data last updated” text.
- **Responsive header**: Changelog/Readme wrap below subheader when layout narrows (e.g. when item cards go 2-wide).
- **Changelog viewer**: Modal with markdown content; supports inline links (e.g. Discord).
- **Readme (metrics) viewer**: Modal with metrics documentation.

---

## API (Implemented)

- `GET /api/items` — Paginated item list; query params for search, sort, filters, grouping, year.
- `GET /api/items/[uuid]` — Single item (detail, OG).
- `GET /api/items/[uuid]/owners` — Ownership list (opted-in only); `limit`/`offset`; returns `total`, `totalAllOwners`, `hasMore`.
- `GET /api/players/[uuid]/username` — Proxy for Minecraft username lookup (avoids CORS).
- `GET /api/og/[uuid]` — Dynamic OG image for an item.

---

## Data & Scripts (Relevant to Implemented Scope)

- **Item data**: `site/data/items.json` (deterministic UUIDs, `_searchText`, `_displayName`).
- **Ownership data**: `site/data/item-ownership.db` (SQLite): `item_ownership` (compressed BLOB), `player_opt_in` (UUIDs only; presence = opted in).
- **Scripts**: e.g. `add-opt-in-players.ts` (add UUIDs/usernames from a text file to `player_opt_in`).

---

## Explicitly Out of Scope (Not Described Here)

- Map views, map APIs, and any map-related UI or data.
