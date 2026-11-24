# The Stalker — target design

**State:** design target, not implemented. **Owner:** Tyr Horror. **Last reviewed:** 2026-11-23. This is the dedicated Stalker design space. [Source analysis](engineering-sources.md) and [implementation/QA roadmap](implementation-roadmap.md) are separate so design decisions are not confused with proposed code.

## Accepted player experience and mechanics

- **Identity:** one persistent, physically present, player-shaped horror creature with behavior reminiscent of a second Minecraft player. It deliberately avoids being seen, rather than functioning as a conventional combat mob or ambient-effects generator.
- **Activation:** it does not start existing/stalking for a player until that player has placed a bed **and successfully set their respawn at it**. Merely placing a bed is insufficient.
- **Initial activity:** it spawns at some distance, gathers actual world resources, crafts useful tools, collects valuables, and establishes a hidden underground base/treasure hoard near the player's spawn bed. The intended proximity is **within roughly two chunks (32 blocks)** of that bed; the exact selection algorithm and safe placement rules are not fixed.
- **Supernatural awareness:** it always knows the current location of its target player and the player's spawn bed (or last known bed if removed). This knowledge does not by itself mean unrestricted teleportation or permanent chunk loading.
- **Autonomous survival:** chopping/mining changes real blocks and creates real drops. Crafting consumes real ingredients through real recipes. Its inventory, equipment, built workstations, bed, and hoard persist. Mined materials and any stolen items must remain physically recoverable rather than silently disappearing.
- **A normal-looking presence:** it walks, mines, opens doors, places appropriate blocks, uses workstations, and occasionally whispers. Footsteps, block-breaking sounds, and partial glimpses are early clues, not obligatory jump scares.
- **Escalation:** as Minecraft days pass, it follows more closely, hides nearer the player, and eventually seeks to touch them. A **roughly five-day** ramp to the first contact attempt is the intended starting design, subject to pacing tests/configuration.
- **Sight/banishment:** when the player catches sight of it, it stops. Sustained, deliberately uncomfortable eye contact banishes it and resets its stalking escalation. Exact required staring duration and visibility thresholds remain to be tuned.
- **Touch penalty:** each successful touch removes **1 maximum-health point** (half a heart, not a full heart). A successful banishment restores the maximum health stolen by that Stalker; the mechanism must not overwrite changes from other mods or effects.
- **Home bed as lifeline:** the Stalker has a real bed in its underground base. With that bed intact, banishment sends it back to its bed, preserving the entity's identity, inventory, hoard, and home while restarting the approach cycle.
- **Destroying its bed:** if banished while its own bed is absent, it is sent to the player's spawn-bed location and tries to establish a new hideout and replacement bed. **A subsequent banishment while still bedless kills that Stalker permanently.** If it successfully establishes a replacement bed before the second banishment, the ordinary bed-protected cycle resumes.
- **Agency:** repeatedly observing/banishing it is valid indefinite defense. Discovering and breaking its bed offers a way to finish the haunting without a conventional boss fight.

## What is specifically NOT settled

- Exact initial spawn range (an earlier 128–256-block figure was a suggestion, **not a locked target**); precise hideout depth/geometry, protection buffers, search retry policy, and what happens when there is no viable location within two chunks.
- Exact five-day distance thresholds, calendar semantics when sleeping/skipping nights, stare duration (8–12 seconds was suggested, not approved as a fixed value), touch cadence/cooldown, health-loss lower bound, and whether touch inflicts ordinary damage as well.
- Which player items may be taken, theft frequency, whether personal inventory versus containers may be accessed, safe recovery, and how claims/locked storage are handled. **Do not implement theft yet** without explicit limits.
- Exactly one Stalker per player versus other multiplayer ownership/concurrency arrangements, PvP/observation by other players, offline target handling, bed moves, lost beds, dimension changes and death, and permanent-death reset policy for new beds.
- Whether resource collection includes fighting mobs, farming, smelting, arbitrary modded recipes/machines, and how it reacts if the player raids its hoard or destroys the base.
- Visual skin/identity, sound frequency, base aesthetics, and how the player discovers the bed vulnerability.
- Packaging, game version, loader, Java/mappings, and compatibility targets. **Minecraft 1.21.1 / NeoForge / Java 21 is a recommended research candidate, not an approved project-wide commitment.**

## Engineering invariants (targets, not claims of implementation)

1. **Physical accounting:** no conjured tools, phantom mining, duplicated drops, missing stolen stacks, or simulated edits to unloaded chunks. Plan while unloaded if needed; only act in loaded chunks.
2. **Safety first:** no destructive actions on player constructions/containers without an approved permission and recovery policy. Block breaking/placement must respect protection and cancellation hooks; use player-context interactions when appropriate.
3. **Bounded work:** limit path searches, target scans, planner recursion, retries, tick budgets, world mutations, and number of active Stalkers. Avoid permanent force-loading for autonomous activity.
4. **One authoritative life record:** server-persisted target identity, linked spawn bed, own bed, base, inventory, hoard pointers, phase, banishment/bedless state, stolen maximum-health ledger, and permanent death. Reconcile after reload rather than duplicating a Stalker.
5. **Interruption-safe:** spotting the Stalker can interrupt any survival task without losing or duplicating resources. On resumption it revalidates world state and inventories.
6. **Mod compatibility:** use actual recipes and item data components, not only hardcoded vanilla item names; handle container insertion/extraction transactionally.

## Status and next steps

No Stalker source or gameplay test exists yet. This page describes **target behavior**, not implementation approval for destructive theft or world modification. The [implementation roadmap](implementation-roadmap.md) separates source-ready investigations, unresolved owner decisions, and pending runtime acceptance. The [source analysis](engineering-sources.md) records exactly which upstream implementations were inspected and where their shortcuts are unsafe.
