# Tyr-Horror — repository-wide agent instructions

These instructions apply to AI coding agents, automation, and contributors working anywhere in **this repository**. Read the [README](README.md) and the relevant [concept notes](docs/concepts/README.md) before changing a feature.

## Relationship to Too Much Magic

These **locally enforceable** rules adapt the general development and safety principles of [Too Much Magic's AGENTS.md](https://github.com/heatwaveqc/Too-Much-Magic-Modpack/blob/main/AGENTS.md) (initial reference: `8f2dd26f208e5138c8740fb7edfad6502bd5fa29`, inspected 2026-11-23). Also consult the [TMM agent contract](https://github.com/heatwaveqc/Too-Much-Magic-Modpack/blob/test/full-integration/AGENTS.md) when evaluating how to improve the process.

**A cross-repository Markdown link does not automatically import AGENTS.md.** Keep the essential rules here. An agent may inspect upstream for process improvements, but must not silently apply new upstream rules. Proposed policy changes get a reviewed change here. In a conflict, the rules in this repository govern Tyr-Horror.

**Do not inherit TMM-specific assumptions:** Progression ownership, modpack/packwiz manifests, named integration branches, approval consoles, exact TMM Minecraft/NeoForge versions, build workflows, mod rosters, or any TMM QA authorization. Treat the repositories as independent.

## Design and task selection

- The owner decides unresolved gameplay, behavior, scope, player impact, balance, technical target, and architecture questions. Record proposals separately from approved design. Do not turn a tentative idea into a fixed mechanic or implementation without approval.
- Owner-attended discussions favor design and decision-making; unattended execution favors already approved, discrete, unblocked work. An explicit owner request to implement takes priority over this preference, subject to safety and ownership.
- Prefer emergent, systemic horror to pure scripted jump scares. Preserve player agency, believable world interaction, and clear bounds on resource use, mob AI, pathfinding, chunk loading, world mutation, persistence, and multiplayer scaling.
- Do not implement irreversible world griefing, destructive inventory operations, or behavior that risks corrupting saves without an explicit design decision, safeguards, and recovery tests.

## One workstream, one live PR by default

- The unit of work is a continuing **feature/workstream**, not a chat message or agent session. For a follow-up, first update the existing issue, branch, and PR.
- **Before any new branch or PR**, inspect live issues, open/merged PRs, relevant branches, integrated code, dependencies, and designated writer. State what existing work can be reused or why a separate PR is genuinely required.
- New PRs are justified for independently reviewable/mergeable changes with a real separate ownership or risk boundary, not to manufacture progress. Keep stacks shallow, identify their dependencies, and reconcile them promptly.
- One designated writer per shared branch. Do not concurrently write to another agent's branch, overwrite unreviewed work, force-push shared history, or restore stale directories blindly.
- After integration, reconcile donor PRs and successors, record provenance, close superseded PRs, and retain unresolved work in the issue/test queue. Do not delete branches until dependent work and exact refs are accounted for.

## Required work-slice status

Maintain three separate inventories in each active issue/PR or in a clearly linked canonical workstream ledger:

1. **Implementation-ready**: approved unimplemented slices with behavior, acceptance criteria, dependencies, writer, and state (ready / in progress / source-ready / integrated).
2. **Design blockers**: precise unresolved owner decisions, affected slices, and choices. Engineering investigation, CI issues, and gameplay testing are not automatically design blockers.
3. **In-game QA queue**: every behavior requiring a real Minecraft session, with prerequisites, expected observations, and state. Use **A** = offline/source checks, **B** = verified in-game command test, and **C** = manual gameplay/setup. An A pass is not a B or C pass.

Keep the ledgers up to date at each decision, code checkpoint, handoff, and integration. A source-ready feature may still await B/C testing; don't claim it is gameplay-tested. Before claiming nothing remains to code, reconcile the ledgers with actual implementation and dependencies.

## Git, CI, builds, and QA

- `main` is the initial accepted baseline. Use short-lived topic branches and reviewed PRs for substantive work. Do not invent permanent integration or subsystem branches without the owner's approval.
- Do not create temporary branches that linger without a PR; aim for a draft or ready PR within 24 hours. Record exact commits, intended target, relevant paths, dependencies, quick-check evidence, and next action.
- Routine quick CI may include formatting, docs checks, source validation, targeted compilation, and unit tests. **Do not configure automatic full Minecraft server boots, client launches, modpack assembly, installer/test ZIP creation, or release publishing** on push, PR, schedule, or other unapproved triggers.
- Each full game boot, expensive gameplay smoke, client/package build, or release requires explicit owner authorization for that run. Do not infer authorization from an instruction to code, merge, or prepare a test plan.
- Never describe a docs check or source compilation as a Minecraft client/server/gameplay PASS. Report what was actually run and what could not be verified.
- Remote branch deletion/retirement must be reviewed against open PR bases/heads, dependencies, and exact SHAs; no automatic blanket pruning or unreviewed destructive cleanup. Ordinary GitHub deletion of a safely merged feature head is acceptable when dependents are preserved.
- Never silently reuse TMM's release tags, CI workflows, credentials, or QA dispatch commands.

## Repository status

As of bootstrap there is **no implemented mod, approved loader/game version, or established CI**. Do not invent commands, module names, passing tests, or an existing architecture. Establish them in independently reviewable steps when approved.

The [Stalker](docs/concepts/stalker.md) and [Liminal](docs/concepts/liminal.md) are separate design concepts under the same umbrella; their eventual packaging, dependencies, and compatibility are **undecided**.
