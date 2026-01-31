# Architecture & Tech Stack

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | Python 3 |
| Parsing | Custom command parser with brace/quote-aware tokenization |
| Data | CSV lookup tables (blocks, entities, sounds, particles) |
| Reference | mcstacker-1.21.10.min.js for target format validation |

---

## High-Level Structure

```
command_converter.py          # Core conversion engine (~7.5k lines)
├── LookupTables              # Loads and manages CSV mappings
├── ParameterConverters       # Block, entity, selector, NBT converters
├── CommandConverter          # Command-specific handlers (execute, summon, etc.)
└── CommandParser             # Minecraft command tokenizer

convert_single_world.py       # Single-world pipeline
batch_convert_worlds.py        # Multi-world processing
extract_commands.py           # Extract from .mca region files
reimport_commands_simple.py   # Write converted commands back
```

---

## Design Decisions

- **Lookup tables over hardcoding:** Block, entity, and sound mappings live in CSVs for maintainability.
- **Modular handlers:** Each command type has its own conversion function (`_convert_execute`, `_convert_summon`, etc.).
- **Recursive NBT:** NBT is converted recursively so nested structures (items in entities, spawn data in spawners) are fully handled.
- **Fallback chain:** Block states → block name → failure logging, so missing mappings are visible and fixable.
