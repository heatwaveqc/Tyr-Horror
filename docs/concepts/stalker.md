# The Stalker — concept register entry

**State:** accepted concept direction; no implementation or approved release target.

The canonical, detailed Stalker documents now live in a dedicated workspace:

- **[Target design and confirmed player-facing mechanics](../stalker/README.md)** — spawning only after setting bed respawn, a physical persistent survival creature, escalating stalking, maximum-health loss and recovery, hiding/eye-contact banishment, own-bed rebirth and bedless permanent death.
- **[Upstream AI engineering references](../stalker/engineering-sources.md)** — inspected Fake Players and MCAi source paths, which architectural patterns appear reusable, license questions and hazardous implementation shortcuts to avoid.
- **[Milestones, decision blockers and QA acceptance](../stalker/implementation-roadmap.md)** — clear implementation slices, source-readiness limits and pending gameplay testing.

The Stalker is a real entity that gathers, mines, crafts and caches physical resources in a hidden underground base near the target player's bed. It prefers to remain unnoticed. Each Minecraft day increases stalking pressure; eventual touching reduces maximum health by one point. The creature stops when spotted and sustained eye contact banishes it, returning lost maximum health.

Banishment returns it to its own bed while that bed exists. If the player has destroyed its bed, banishment moves it to the player's spawn-bed location to seek a new hideout. A further banishment while still bedless ends the Stalker permanently. Exact timing, distances, multiplayer rules, grief/theft boundaries, loader target and implementation details remain explicitly tracked in the linked canonical files.

**Safety boundary:** the concept's presence here does not authorize unattended theft, destructive block operations, chunk loading or a Minecraft boot. Review the local [AGENTS.md](../../AGENTS.md) before implementation.
