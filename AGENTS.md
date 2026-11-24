# Tyr-Horror — agent bootstrap

Shared repository: heatwaveqc/Tyr-Development-Standards
Revision: 17891222a30199563d36df419303820efb8fbe5a
Profile: minecraft

Before mutations retrieve [shared AGENTS.md](https://github.com/heatwaveqc/Tyr-Development-Standards/blob/17891222a30199563d36df419303820efb8fbe5a/AGENTS.md), applicable routes (including profiles/minecraft.md), and local instructions at the pinned revision via connector or exact local snapshot. Read once per session and again when scope or revision changes. If unavailable, report blocker and limit affected work to nonmutating investigation. Upgrade the pin only through a reviewed consumer PR. No automatic inheritance or authorization from other repositories.

## Local contract

- **Purpose and scope:** experimental systemic Minecraft horror mods. Preserve player agency; bound AI, pathfinding, chunk loading, world mutation, persistence and multiplayer costs. Irreversible griefing, destructive inventory operations or save-corruption risks require explicit design approval, safeguards and recovery tests.
- **Design paths:** [README](README.md) and [concept register](docs/concepts/README.md). The Stalker and Liminal are separate concepts; packaging, dependencies, game/loader version and architecture remain undecided. Tentative ideas are not implementation approval.
- **Writers and integration authority:** one writer per workstream under shared standards; the owner or explicitly delegated integration writer may merge reviewed, approved scope. Do not infer delegation from another repository.
- **Permanent branches and targets:** main only; short-lived topic branches target main. New permanent branches require owner approval.
- **Quick checks:** documentation/source validation, targeted compilation and unit tests when available. No CI or mod source exists at bootstrap; report actual evidence.
- **Build/runtime/release authorization:** each full Minecraft boot (server or client), expensive gameplay smoke, client/package build, assembly or release requires explicit per-run owner authorization. No automatic triggers for these; no authorization inherited from TMM.
- **Owner-approved exceptions:** none.
