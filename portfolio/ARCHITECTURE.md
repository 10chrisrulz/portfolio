# Architecture (Implemented Scope)

High-level structure of the MineZ Item Flow Tracker. Map-related endpoints and UI are not included.

---

## Frontend

- **Framework**: Next.js 16 App Router, React 19, TypeScript.
- **Main page** (`app/page.tsx`): Client component; URL sync for `?item=<uuid>`; holds search/filter/sort state; uses `useItemsInfinite` for paginated items and `ItemDetail` for selected item.
- **Item page** (`app/item/[uuid]/page.tsx`): Server-rendered item detail and metadata for deep links and OG.
- **Key components**:
  - `Header`: Site title, subheader, Changelog/Readme buttons, last-updated; responsive wrap.
  - `SearchFilters`: Search inputs, checkboxes, group-by, currently-exists, sort.
  - `ItemList` + `ItemCard`: Infinite list of cards; click opens detail.
  - `ItemDetail`: Full stats, lore, chart, Copy/Readme/Owners; responsive metrics/chart layout.
  - `OwnersViewer`: Sheet modal; fetches owners, resolves usernames (cached), shows skull/name/quantity.
  - `ChangelogViewer` / `ReadmeViewer`: Sheet modals with markdown (links supported in changelog).
  - `MonthlyChart`: Recharts-based monthly created/removed; year selector.

---

## Data Flow

1. **Items**: Loaded/processed in `GET /api/items` from `site/data/items.json` (with `_searchText`, `_displayName`, deterministic UUID). Filtering, sorting, and pagination are done in-memory; results and `hasMore` returned to client.
2. **Single item**: `GET /api/items/[uuid]` returns one item for detail view and OG.
3. **Ownership**: `GET /api/items/[uuid]/owners` reads `site/data/item-ownership.db` (read-only): decompresses BLOB per item row, filters to UUIDs present in `player_opt_in`, then paginates (`limit`/`offset`). Returns `owners`, `total`, `totalAllOwners`, `hasMore`.
4. **Usernames**: Client calls `GET /api/players/[uuid]/username` (proxies Mojang); responses can be cached (e.g. sessionStorage) to avoid repeated calls.

---

## API Summary

| Endpoint | Purpose |
|----------|--------|
| `GET /api/items` | List items with search, sort, filters, grouping, pagination. |
| `GET /api/items/[uuid]` | Single item by UUID. |
| `GET /api/items/[uuid]/owners` | Opted-in owners for item; `limit`, `offset`; `total`, `totalAllOwners`, `hasMore`. |
| `GET /api/players/[uuid]/username` | Minecraft username for UUID (proxy). |
| `GET /api/og/[uuid]` | Dynamic OG image for item. |

All UUID path params are validated (format); `limit`/`offset` are bounded to prevent abuse.

---

## Data Storage

- **items.json**: Static JSON; item definitions and precomputed metrics; deterministic UUIDs for stable linking and ownership join.
- **item-ownership.db**: SQLite (better-sqlite3, read-only in API):
  - `item_ownership`: e.g. `item_uuid`, `ownership_data` (compressed BLOB of player UUID → quantity).
  - `player_opt_in`: list of player UUIDs; presence = opted in. No PII; used only to filter what is returned by `/api/items/[uuid]/owners`.

---

## Security (Relevant to Implemented Scope)

- **Ownership**: Non-opted-in owners are never returned; filtering is done server-side.
- **API**: UUID validation on all UUID path/query usage; `limit`/`offset` clamped.
- **DB**: Opened read-only for API; no user-supplied SQL; parameterized queries only.

---

## Build & Run

- **Dev**: `npm run dev` (Next.js dev server).
- **Prod**: `npm run build` then `npm run start`.
- **Data**: Ensure `site/data/items.json` and `site/data/item-ownership.db` exist and are in the format expected by the API (see main repo README / data scripts).
