# The Stalker — upstream source inspection and reuse strategy

**Inspected:** 2026-11-23. Source review only; upstream code was **not compiled or playtested**. See [target design](README.md) and [implementation roadmap](implementation-roadmap.md).

## Fake Players — primary execution architecture reference

- **Upstream:** https://github.com/duzos/fakeplayer
- **Relevant source branch:** https://github.com/duzos/fakeplayer/tree/ver/1.21.1 (important: use `ver/1.21.1`, not the default branch for the inspected Java implementations).
- **Target branch metadata:** `gradle.properties` declares Minecraft 1.21.1, NeoForge 21.1.228, Java 21, and `LGPL-3.0-only`. Multiloader with `common/` and `forge/` modules.
- **Licensing:** LGPL-3.0-only applies to direct source reuse. Any copying needs notice/license review and compatibility compliance. Prefer learning from architecture or isolated compliant reuse over importing its whole runtime.

Inspected files (paths relative to the branch):

| Upstream file | What it demonstrates | Reuse decision |
| --- | --- | --- |
| [`entities/ai/JobExecutor.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/JobExecutor.java) | Job interface and pause/resume/serialization boundary | Adapt pattern; new Stalker task contract |
| [`entities/ai/MinerJobExecutor.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/MinerJobExecutor.java) | Mining phases, reachable stand positions, block cracks, tool checks, return/deposit and throttling | Adapt finite-state executor; replace world mutation |
| [`entities/ai/builder/BlockBreaker.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/builder/BlockBreaker.java) | Progressive mining, drop fit preflight, tool damage | Study only; implement claim-safe player-context breaking |
| [`entities/ai/CrafterJobExecutor.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/CrafterJobExecutor.java) | Real inventory consumption/output, walking to crafting table, visual crafting, deposit phases and rollback of partially fetched inputs | Adapt job sequencing, not exact crafting shortcuts |
| [`entities/ai/requests/RecipeGraph.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/requests/RecipeGraph.java) | Crafting recipe index by output, ingredient alternatives, reload detection, skip unsafe remainder recipes | Strong candidate for independent implementation |
| [`entities/ai/requests/CraftPlan.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/requests/CraftPlan.java) | Stock-aware recursive prerequisites, cycle detection, depth cap 12 and step cap 16 | Strong model for deterministic bounded planner |
| [`entities/ai/BuilderJobExecutor.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/BuilderJobExecutor.java) | Incremental blueprint cells, reachability, material accounting, multi-block placement | Adapt phases for a small underground hideout; NOT raw placement calls |
| [`entities/ai/storage/ExternalStorage.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/ai/storage/ExternalStorage.java) | Transactional modded storage inserts/extracts; refuses direct slot writes | Strong model for recoverable hoard transfers |
| [`entities/inventory/FakePlayerInventory.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/inventory/FakePlayerInventory.java) | Persistent container with state-sensitive insertion guards | Adapt accounting/serialization concepts |
| [`entities/goal/SleepInBedGoal.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/goal/SleepInBedGoal.java) | Bed search, POI claiming, pathing, sleep/wake/reload | Adapt physical bed behavior; own-bed banishment is new |
| [`entities/FakePlayerEntity.java`](https://github.com/duzos/fakeplayer/blob/ver/1.21.1/common/src/main/java/dev/duzo/players/entities/FakePlayerEntity.java) | `PathfinderMob` entity with inventory, job execution, visual sync and persistence | Prefer custom Stalker `PathfinderMob`, not wholesale entity copy |

**Critical audit finding:** `BuilderJobExecutor.place()` uses `level.setBlock` and explicitly comments that claim-protection events are not fired; its breaking path has the same caveat. `BlockBreaker.breakIntoInventory()` computes `Block.getDrops` before `level.destroyBlock(..., false, entity)`. Do **not** transplant these APIs as a general player-equivalent mining/placement implementation. Re-check player protection, actual break success, drop generation, and item transactions through supported NeoForge hooks.

## MCAi — broader production-chain planning reference

- **Upstream:** https://github.com/Apocscode/MCAi
- **Source:** https://github.com/Apocscode/MCAi/tree/master/src/main/java/com/apocscode/mcai
- **Branch seen:** `master`; code-search results exposed revision `0416991c9e7ff8b171a3c66bd1ffb67c793cbc33` at inspection. Its `build.gradle` uses NeoForge ModDev and Java 21; Minecraft/NeoForge exact values should be verified from its properties before adoption.
- **License caution:** a top-level `LICENSE` lookup returned 404 during inspection. An earlier README-based MIT claim was not independently confirmed here. **Do not copy MCAi code** until a valid license grant is verified.

Inspected source:

| Upstream file | What it demonstrates | Reuse decision |
| --- | --- | --- |
| [`ai/planner/RecipeResolver.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/ai/planner/RecipeResolver.java) | Output indexes for crafting, furnace/heat recipes and stonecutting; recursive dependency tree and raw-material leaves, depth capped at 10 | Adapt idea to extend Fake Players-style crafting graph; don't copy hardcoded classifications |
| [`ai/tool/CraftItemTool.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/ai/tool/CraftItemTool.java) | End-to-end dependency planning, gathering/task continuation, crafting access checks | Study sequencing, avoid service/AI/chat coupling and hand-built outputs |
| [`task/CompanionTask.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/task/CompanionTask.java) and [`task/TaskManager.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/task/TaskManager.java) | Queue, lifecycle, failure continuation, cleanup | Implement independent local deterministic scheduler |
| [`task/MineOresTask.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/task/MineOresTask.java) and [`task/BlockHelper.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/task/BlockHelper.java) | Resource search and block-action helpers | Use task concept; rewrite world mutation and harvest |
| [`logistics/HomeBaseManager.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/logistics/HomeBaseManager.java) | Craft/place home workstations from actual inventory; scans loaded chunks | Adapt infra prerequisites; new terrain-safe underground site planner |
| [`CompanionChunkLoader.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/CompanionChunkLoader.java) | Force-loads a five-chunk cross during active jobs | Explicitly **do not reuse** for autonomous Stalker behavior |
| [`entity/CompanionEntity.java`](https://github.com/Apocscode/MCAi/blob/master/src/main/java/com/apocscode/mcai/entity/CompanionEntity.java) | PathfinderMob, movement goals, item pickup, persisted ownership/inventory and owner tracking | Reference entity/goal patterns only |

**Critical audit finding:** `BlockHelper.breakBlock()` computes drops before destroying the block and inserts those drops without checking that destruction succeeded. This risks item duplication if breaking is canceled. `CraftItemTool` also builds some output stacks from item/count rather than running exact recipe semantics. Stalker execution must not adopt either behavior unchanged.

## Proposed reuse architecture (proposal, not yet code)

```text
StalkerDirector (player/bed awareness, escalation, observation, banishment)
  -> StealthPlanner / SurvivalPlanner (goals and interruption)
  -> Bounded JobScheduler (plan, execute, pause, resume, recover)
  -> ValidatedActionExecutor (navigation, break/place, craft/smelt, transfer)
  -> Persistent StalkerRecord + physical entity/inventory/hoard
```

Choose an autonomous `PathfinderMob` with real equipment and inventory; use a scoped NeoForge player-context action only where needed for world-event compatibility. Use a pure deterministic recipe/dependency planner, capped and cached per recipe reload; extend with data-driven resource acquisition leaves. Do **not** introduce an external language-model service or required Fake Players/MCAi runtime dependency.

This is an engineering recommendation grounded in source review, **not** a statement that the integration, API compatibility, or licensing of copied source has been verified. Before writing code, inspect the final targeted NeoForge mappings and permissions/event hooks and test failure/cancellation cases.
