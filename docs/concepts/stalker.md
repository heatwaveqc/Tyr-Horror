# The Stalker — working design

**Status:** brainstorming/design, no implementation yet. Name is provisional.

## Accepted direction from concept discussions

- The Stalker is a **physical, persistent, player-like creature**, not merely an ambient sound/effect generator. It should behave like an autonomous second Minecraft player while trying to stay unnoticed.
- It should **not begin stalking an individual player before that player places a bed and sets their spawn**. After that trigger, it can appear somewhere in a large radius, gather materials, and establish a hidden base within a large but bounded distance of the player's spawn. Exact ranges are undecided.
- It should perform **real world interactions** where appropriate: gathering resources, mining blocks, collecting items, crafting useful tools, and establishing a hidden base/treasure hoard.
- Resource and theft outcomes should be physical and persistent: mined blocks and taken valuables are stored in the Stalker's inventory/hoard, not silently deleted or conjured without a design reason.
- Early behavior emphasizes remaining near the player while avoiding detection. Hints may resemble the activity of a second player: distant footsteps, mining/blocks breaking, and occasional whispers.
- Behavior escalates over time toward sneaking closer and eventually touching the player; touches can drain health.
- When noticed, the Stalker stops. Sustained, uncomfortably long eye contact can banish it and reset or interrupt stalking pressure; exact reset mechanics remain to be specified.

## Design constraints

- The Stalker must obey bounded simulation costs. Any offscreen resource-gathering simulation, pathfinding, block changes, container transactions, chunk tickets, and persistent state must be explicitly designed to prevent lag and save corruption.
- Player inventories, containers, and bases need safeguards against destructive griefing or irrecoverable theft. Do not implement theft or block destruction just because the concept mentions physical interaction; first settle limits, ownership, recovery, and multiplayer behavior.
- The creature's power should come from believable actions and its reluctance to be observed, not omniscience or gratuitous unavoidable attacks.

## Open questions

- Exact spawning/base radius and spawn-search algorithm; whether the entity is unique per player, globally shared, or otherwise coordinated in multiplayer.
- How it obtains tools/materials without permanently loading distant chunks, and what operations are simulated vs actually performed.
- How the hidden base is placed relative to protected builds, spawn safety, and world terrain.
- Which items may be stolen, how often, when they become recoverable, and how to handle private/modded inventories.
- Rules for repeated sightings, long eye contact, banishment, death, respawn, progression reset, and inactivity.
- Dimension transitions, player logout, bed relocation, death, world backups, and server restarts.
- Visual identity, audio frequency, difficulty tuning, and how players discover counterplay.

## Implementation and QA

There is no source code yet. Derive discrete implementation-ready slices and B/C acceptance cases **after** resolving design-sensitive behavior. Track work in GitHub issues/PRs under [AGENTS.md](../../AGENTS.md).
