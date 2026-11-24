# The Stalker — implementation sequence and acceptance ledger

**Status:** planning only. No source, test harness, runtime gameplay, or approved build configuration exists yet. [Design](README.md) | [Upstream code study](engineering-sources.md). This page records the three work inventories until engineering issues are opened; do not treat design documentation as code completion.

## Recommended technical candidate (not committed)

Investigate standalone Minecraft **1.21.1 / NeoForge / Java 21** to align with inspected sources. Confirm mappings, NeoForge build, mod ID, data version and dependency rules before scaffold. Separate code ownership from Too Much Magic and from other Tyr Horror concepts (Liminal).

## Milestone sequence

| Milestone | Deliverable | Static acceptance (A) | State |
| --- | --- | --- | --- |
| M0 — design/source record | Canonical mechanic contract, upstream file references, failure audit | Documents crosslinked; unresolved decisions labeled | In progress (docs PR) |
| M1 — minimal skeleton | Entity registration, server persistence, target UUID and bed tracking | Compilation, serial roundtrip, duplicate reconciliation tests | Awaiting technical target approval |
| M2 — stalking loop | Player awareness, cover/navigation selection, observation freeze, gaze timer, staged approach, banishment | Deterministic state-machine/visibility unit tests | Awaiting M1; tuning decisions |
| M3 — survival vertical slice | Start empty; chop logs; craft planks, sticks, table, wooden pickaxe; mine stone; craft stone pickaxe; deposit excess to real chest | Recipe accounting, tool correctness, canceled break, overflow, task interrupt/reload tests | Awaiting M1; safe action design |
| M4 — base and lifeline | Site choice within target bed radius, safe underground excavation, actual bed/chests/workstations, valid-bed return, bedless retreat and permanent-death path | Protection/claims checks, bed-disappearance races, persistence and rollback tests | Awaiting M2/M3; placement approval |
| M5 — expanded survival | Smelting/iron tools, bounded recipe acquisition, hoard sorting and incremental construction | Recipe remainder, modded container and performance tests | Awaiting M3/M4 |
| M6 — theft and compatibility | Explicitly bounded player valuables theft with guaranteed recovery semantics | Unauthorized item access prevented, rollback and crash consistency tests | **Design-blocked** |
| M7 — gameplay acceptance | Single-player and multiplayer manual runs; tuning and player counterplay | Explicitly authorized B/C run receipts and defects | Awaiting source-ready build |

**First implementation proof:** with a blank inventory and accessible loaded wood/stone, autonomously produce and use a wooden pickaxe, mine stone, craft a stone pickaxe, and put surplus items in a *real* chest. The result must be reconcilable through block changes + inventory transactions, with no conjured inputs, no dupe, no dropped state on interruptions.

## 1. Implementation-ready / engineering work

| Slice | Scope/acceptance | Owner | Dependency | PR | State |
| --- | --- | --- | --- | --- | --- |
| Documentation and upstream research | Accepted mechanics, explicit unknowns and source-level notes on both upstreams | Tyr Horror documentation writer | None | Documentation PR | In progress |
| Core deterministic planner spike | Pure recipe graph and task plan tests; no world mutation; prove bounded cycles/remainders | Unassigned | Tech target + license strategy | None | Candidate after owner chooses version |
| Safe action executor investigation | Determine correct NeoForge hooks, FakePlayer event context and rollback semantics; offline tests | Unassigned | Tech target | None | Research candidate |
| Persistence schema sketch | Versioned owner/stalker/bed/inventory/health-ledger model and duplicate reconciliation tests | Unassigned | Tech target + multiplayer ownership decision | None | Candidate |

No gameplay code has started. Implementation milestones beyond investigations are **not automatically authorized** by this design record.

## 2. Owner design decisions that block specific slices

- **Version and packaging:** choose Minecraft, loader, Java and mod boundary (blocks initial scaffold and direct compatibility test).
- **Ownership/multiplayer:** one Stalker per target? whose gaze counts? offline targets and cross-dimension behavior? (blocks definitive persisted identity and targeting tests).
- **World mutation permissions:** exact allowed terrain, existing structures, claim integrations, distance protections, recovery and obstruction behavior (blocks base excavation and autonomous breaking in player worlds).
- **Theft contract:** inventory vs chests, filters and exclusions, rate/cooldowns, consent/config defaults, overflow/recovery on entity death (blocks M6).
- **Banishment edges:** invalid/moved/destroyed player bed, replacement Stalker bed site, death/recreation policy, missing chunks, minimum health and recovery when player attributes change (blocks full lifecycle acceptance).
- **Tuning:** exact initial spawn radius, five-day target semantics during sleeping, gaze duration and contact frequency (blocks balance acceptance, not independent planner work).

## 3. Runtime acceptance queue (all pending; no run authorization inferred)

- **Bed activation:** no entity before valid spawn-setting; exactly expected instance after event; survives save/restart and bed relocation policy.
- **Stealth:** hidden movement generates real audio/blocks; sight freezes; sustained eye contact returns home and restores correct max-HP ledger; phase resets.
- **Survival pilot:** physical wood/stone → pickaxes → real chest, exact material accounting through interruption, full inventory and cancellation.
- **Bedless finish:** destroy own bed; first banishment returns to player bed and starts rebuilding; second while still bedless permanently ends haunting; replacement bed before second banishment restores normal cycle.
- **Safety:** claim/protection denied break and placement, locked/modded containers, player-built walls, lava/fall hazards, unload/reload, dimension/logout and multiple players.
- **Performance:** active Stalker limits, no unbounded path scan or force-loaded chunk chain, stopped work when unloaded; profiling on agreed machine/server.

**QA gates:** A = static/offline/source/build checks where allowed; B = verified in-game commands; C = manual gameplay/client/multiplayer. Full server/client boots, assembly and packaging require explicit owner authorization per local `AGENTS.md`. Mark source-ready, integrated, and gameplay-accepted separately.

## Handoff rule

One coherent Stalker workstream and one live PR per code slice; reuse an existing open PR if scope/ownership matches. Record base/head SHA, test receipts, design blockers, exact source paths, and next action. Do not create a new PR merely because a new chat turn begins.
