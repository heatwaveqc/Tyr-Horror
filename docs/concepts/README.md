# Horror concept register

This is a **design record**, not a development-completion claim. Keep accepted decisions, tentative possibilities, and unanswered questions distinguishable. Proposed mechanics are not implementation instructions until approved.

## Current concepts

- [The Stalker](stalker.md) — persistent, resource-gathering, player-like stalking entity with an underground base and bed-based banishment/permanent death. Its dedicated [design space](../stalker/README.md) includes [upstream code sources](../stalker/engineering-sources.md) and an [implementation roadmap](../stalker/implementation-roadmap.md).
- [Liminal](liminal.md) — a layered, Backrooms-inspired dimension with themed biomes and an evolving mold threat.

## Rules for updating this register

- Record clear owner decisions as **Accepted design**; unresolved ideas remain **Provisional** or **Open questions**.
- Prefer observable acceptance conditions over implementation guesses. Changes involving griefing, inventory theft, forced death, persistent chunk loading, or other high-impact behavior need an explicit owner decision.
- Track actual coding in issues and PRs using the three inventories required in [AGENTS.md](../../AGENTS.md): implementation-ready work, design blockers, and in-game QA.
- Neither concept currently has implemented source code, a finalized Minecraft/loader version, or an approved release configuration.
