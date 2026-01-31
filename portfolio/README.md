# MineZ Item Flow Tracker — Portfolio Overview

Part of this portfolio → [Portfolio index](../../README.md)

A Next.js web application for the MineZ game mode that tracks item statistics, usage rates, and flow data. Users can browse, search, filter, and inspect detailed metrics for in-game items, including optional public ownership data for players who opt in.

---

## Purpose

- **Audience**: MineZ players and data enthusiasts.
- **Data**: Snapshot-based item metrics (creation, removal, active counts) from 2024–2025, plus optional item-ownership data stored in SQLite.
- **Scope**: Implemented features only (item browsing, search, filters, detail view, ownership modal, charts, OG images, changelog/metrics docs). Map-related features are not part of this scope.

---

## Tech Stack

| Layer | Technologies |
|-------|--------------|
| **Framework** | Next.js 16 (App Router), React 19, TypeScript |
| **Styling** | Tailwind CSS, shadcn/ui (Radix), lucide-react |
| **Data / State** | TanStack React Query, infinite scroll |
| **Backend / Data** | Node.js API routes, better-sqlite3 (read-only), static JSON |
| **Charts** | Recharts |
| **Images** | @vercel/og (dynamic OG images), mc-heads.net (player skulls) |

---

## Implemented Features (Summary)

- **Search & filters**: Multi-field search (name, lore, enchantments), hide heal kit/component bag/player heads, group by name, “currently exists” filter.
- **Sorting**: By name, usage rate, yearly created, total seen, currently active, total removed.
- **Item list**: Infinite scroll, color-coded Minecraft names, lore rendering, skull textures for skull items, share links (disabled for grouped items).
- **Item detail**: Expanded stats, lore, monthly chart with year selector, skull texture, Copy/Readme/Owners actions.
- **Item ownership**: Opt-in only. “Owners” button opens a modal with player heads, usernames, and quantities; initial load of 15 with support for more; description shows “X of Y owners opted in.”
- **Charts**: Monthly created/removed with responsive layout and year selector.
- **OG images**: Dynamic per-item Open Graph images for sharing.
- **Docs**: In-app Changelog and Metrics README viewers.

See [FEATURES.md](./FEATURES.md) for a detailed feature list.

---

## Project Structure (High Level)

```
app/                    # Next.js App Router
  api/                  # API routes (items, owners, players/username, og)
  item/[uuid]/          # Item detail page
  page.tsx              # Main listing page
  layout.tsx, providers, globals
components/             # React UI
  ItemCard, ItemDetail, ItemList, SearchFilters, Header
  OwnersViewer, ChangelogViewer, ReadmeViewer
  MonthlyChart, MetricTooltip, ui/*
hooks/                  # useItemsInfinite (infinite scroll)
lib/                    # Parsing & utilities (item, lore, skull, player, enchantment)
types/                  # TypeScript (item, ownership)
public/                 # CHANGELOG.md, README_METRICS.md
site/data/              # items.json, item-ownership.db (SQLite)
scripts/                # Data/opt-in utilities (add-opt-in-players, etc.)
```

See [ARCHITECTURE.md](./ARCHITECTURE.md) for APIs and data flow.

---

## Running the Project

**Requirements**: Node.js (LTS), npm.

```bash
npm install
npm run dev
```

**Production:**

```bash
npm run build
npm run start
```

**Data**: Place `items.json` and `item-ownership.db` under `site/data/` as expected by the app (see main repo README for data pipeline).

---

## Links (fill in for your portfolio)

- **Live site**: https://minez-item-tracker.shotbow.net/

---

## Metrics Reference

Formulas for Total Seen, Currently Active, Total Removed, Usage Rate, and yearly/monthly breakdowns are documented in the in-app Metrics README.
