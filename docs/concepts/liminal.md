# Liminal — working design

**Status:** brainstorming/design, no implementation yet. Name and dimension name are provisional.

## Accepted direction from concept discussions

- A separate, enormous Backrooms-inspired dimension tentatively called **The Rooms**: effectively infinite in horizontal extent, like the Overworld.
- Vertical layout follows a proposed repeating **8-block pitch**, with roughly **6 blocks of usable room height** and separating floor/ceiling layers. The top level's ceiling is near build height.
- Most entry methods put the player on the top level. Descending generally makes exploration more dangerous, unusual, and surreal.
- The player **cannot set their spawn inside The Rooms**.
- Generate both **themed biomes/regions** (examples: bathrooms, apartments, offices, mall spaces) and **pseudo-structures** rather than choosing only one system.
- Carpet has special water behavior: placing water onto/into carpet can soak it instead of behaving as ordinary free-flowing water; continued moisture can produce mold.
- Mold can develop a hidden **mold heart** within walls. A heart can animate or sustain a threatening body in a manner reminiscent of the Creaking; killing a body is not necessarily a permanent solution because mold can spread and establish more hearts.

## Design constraints

- Dimension generation must have robust vertical seams and chunk boundaries; avoid expensive cross-chunk rescans or runaway recursive mold growth.
- Mold propagation, heart creation, and monster activation require explicit tick budgets, loaded-chunk behavior, cleanup rules, and prevention of infinite growth.
- Different floors and regions should have navigable logic and readable environmental clues without losing the feeling of disorientation.

## Open questions

- Entry and escape mechanisms; whether different methods lead to different floors; how players recover after death.
- Exact pitch/room-air/floor/ceiling allocation at minimum and maximum build heights.
- Floor-by-floor biome allocation, pseudo-structure rules, rarity, transitions, and escalating hazards.
- Mold wetness states, the trigger for hostility, heart limits, propagation range/rate, and whether players can permanently clear infestations.
- Other inhabitants, loot/economy, protection from softlocks, and compatibility with multiplayer/other mods.
- Final Minecraft version, loader, module layout, and whether Liminal ships separately from The Stalker.

## Implementation and QA

There is no source code yet. Split the accepted design into approved implementation slices when the technical baseline is chosen, and track worldgen and in-game behavior tests separately from source checks per [AGENTS.md](../../AGENTS.md).
