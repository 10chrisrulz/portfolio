# Challenges & Solutions

## 1. Execute Command Restructuring

**Challenge:** The `execute` command changed from linear (`execute @e x y z command`) to chainable (`execute as @e at @s positioned x y z run command`). Nested `execute` blocks and mixed formats had to be handled.

**Solution:** Implemented context-aware parsing that detects pre-converted subcommands (e.g., `positioned ~ ~ ~ run summon ...`), extracts the inner command, converts it recursively, and rebuilds the chain.

---

## 2. NBT Components vs. Tags

**Challenge:** 1.12 uses NBT tags (e.g., `display:{Name:...,Lore:[...]}`). 1.21 uses component-style keys (`minecraft:custom_name`, `minecraft:lore`) with different structures. Item NBT also moved from `tag` to `components`.

**Solution:** Built a recursive NBT converter with:
- Mappings from legacy keys to component names
- Special handling for `SkullOwner` → `minecraft:profile`
- Lore conversion from list-of-lists to flat array with `extra` for multi-component lines

---

## 3. Lore Line Structure

**Challenge:** Lore was being split incorrectly—one original line (e.g., `"§4A strange item from §cFrostbain Catacombs§4."`) became multiple entries instead of a single line with multiple components.

**Solution:** Switched to flat lore format: one array entry per original line, with `extra` used when a line has multiple text components. Each quoted string in `Lore:[...]` maps to one tooltip line.

---

## 4. Spawner SpawnData Format

**Challenge:** Spawner NBT in 1.12 used `SpawnData:{id:zombie}`. 1.21 expects `SpawnData:{entity:{id:"minecraft:zombie"}}` with a static `entity` wrapper.

**Solution:** Added SpawnData handling in block NBT conversion: detect `SpawnData:{...}`, wrap the payload in `entity:`, and convert the entity ID with proper namespacing.

---

## 5. Setblock NBT Loss

**Challenge:** `/setblock` was dropping NBT when the command had both an action (`replace`/`destroy`/`keep`) and block NBT. Output looked like `setblock x y z block destroy` with NBT missing.

**Solution:** Updated argument parsing to treat NBT as the next token after the action. Also fixed brace stripping: `strip('{}')` was removing nested `}`; replaced with `[1:-1]` to remove only the outer braces.

---

## 6. Block State Mapping Accuracy

**Challenge:** Block data values (e.g., button facing/face/powered) map to block states in non-obvious ways. Incorrect mappings produced invalid blocks.

**Solution:** Curated `ID_Lookups_with_states.csv` with explicit `face`, `facing`, and `powered` (or equivalent) for directional blocks like buttons and levers. Stone and wooden buttons share the same data→state mapping.

---

## 7. Skull / Player Head Conversion

**Challenge:** `minecraft:skull` with `Damage:3` or `SkullOwner` NBT needed to become `minecraft:player_head` with `minecraft:profile` in different contexts (items, entities, block entities).

**Solution:** Applied skull→player_head conversion in item NBT, entity NBT, and block NBT paths. Ensured `SkullOwner` is transformed to `minecraft:profile` with the correct structure.

---

## 8. Command Parsing with Nested Braces

**Challenge:** NBT in commands uses nested `{}` and `[]`. Naive space-splitting broke on `{SpawnData:{id:zombie}}`.

**Solution:** Implemented brace-level–aware tokenization: spaces only split when not inside quotes, braces, or brackets. Keeps full NBT strings as single tokens.
