# Portfolio

A selection of projects spanning full-stack web apps, data pipelines, and game-modding tooling — with an emphasis on clear architecture, deterministic data, and maintainable tooling.

---

## Projects

### 1. MineZ Item Flow Tracker — *Public-facing website*

A **Next.js** web app for the MineZ game mode that tracks item statistics, usage rates, and flow data. Players and data enthusiasts can browse, search, filter, and inspect detailed metrics for in-game items, with optional public ownership data for players who opt in.

| | |
|--|--|
| **Stack** | Next.js 16 (App Router), React 19, TypeScript, Tailwind, shadcn/ui, TanStack Query, Recharts, SQLite (read-only API), better-sqlite3 |
| **Highlights** | Infinite scroll, multi-field search & filters, item detail views with monthly charts, opt-in ownership modal, dynamic OG images, in-app Changelog & Metrics docs |
| **Live** | [minez-item-tracker.shotbow.net](https://minez-item-tracker.shotbow.net/) |

**Docs:** [README](portfolio/README.md) · [Architecture](portfolio/ARCHITECTURE.md) · [Features](portfolio/FEATURES.md)

---

### 2. MineZ Data Scraping — *Data pipeline for the tracker*

A **Python** data pipeline that ingests player save data, derives item-tracking metrics, and produces structured outputs (SQLite, JSON, CSV) that power the item tracker website. Focus: reliability, deterministic item IDs, and opt-in–aware ownership data.

| | |
|--|--|
| **Stack** | Python 3, SQLite, CSV/JSON config, gzip-compressed blobs |
| **Highlights** | Deterministic item UUIDs (stable across runs), opt-in ownership filtering, compressed ownership storage, alignment with item-tracking scripts and website |
| **Outputs** | `item-ownership.db`, `items.json`, CSV exports |

**Docs:** [Overview](projects/data-scraper/README.md) · [Data sources & flow](projects/data-scraper/DATA_SOURCES.md) · [Item ownership pipeline](projects/data-scraper/ITEM_OWNERSHIP.md)

---

### 3. Minecraft 1.12 → 1.21.10 Command & World Converter — *Migration tooling*

A **Python** tool for migrating Minecraft worlds and command blocks across major version boundaries (1.12 → 1.21.10). Converts commands, NBT, selectors, and block/entity IDs so custom maps and command-block systems can be brought forward across 9+ years of engine changes.

| | |
|--|--|
| **Stack** | Python 3, custom command parser (brace/quote-aware), CSV lookup tables (blocks, entities, sounds, particles), mcstacker reference for target format |
| **Highlights** | ~40+ command types, recursive NBT → component conversion, selector translation, batch world processing, failure logging |
| **Primary script** | `command_converter.py` (~7.5k lines) |

**Docs:** [Overview](projects/command-converter/README.md) · [Architecture](projects/command-converter/architecture.md) · [Features](projects/command-converter/features.md) · [Challenges & solutions](projects/command-converter/challenges.md)

---

## How they connect

- The **data scraper** produces `items.json` and `item-ownership.db` from player saves and opt-in lists.
- The **item tracker** consumes that data via read-only APIs and serves the public site.
- The **command converter** is standalone tooling for Minecraft map/server migration (no direct dependency on the MineZ stack).

---

## Summary

| Project | Role | Main tech |
|--------|------|-----------|
| Item Tracker | Public website | Next.js, React, TypeScript, SQLite |
| Data Scraper | Pipeline for tracker data | Python, SQLite, CSV/JSON |
| Command Converter | Minecraft 1.12→1.21 migration | Python, custom parser, CSV lookups |

For detailed run instructions, APIs, and feature lists, see the linked docs under each project.
