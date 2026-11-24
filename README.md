# Tyr-Horror

A repository for experimental **Minecraft horror mods** and their design, implementation, and tests.

**Status:** concept/design stage. No mod source, build pipeline, game-version target, or loader target has been accepted yet. The repository name is a working umbrella name, not a commitment to a single released mod.

## Current concepts

- **The Stalker** — an elusive, player-like entity that starts its existence after a player has placed a bed and set spawn. It gathers real resources, creates a hidden base and hoard, stalks the player while avoiding sight, and escalates its behavior over time.
- **Liminal** — a vast, layered, Backrooms-like dimension tentatively called *The Rooms*, featuring increasingly strange and dangerous floors, themed biomes/structures, and an evolving mold ecosystem.

See [the concept index](docs/concepts/README.md) for current decisions, tentative ideas, and design questions. **Concept documents are not proof that the features exist in code.**

## Development

Read [AGENTS.md](AGENTS.md) before contributing. It contains self-contained safety and workflow rules adapted from [Too Much Magic's agent contract](https://github.com/heatwaveqc/Too-Much-Magic-Modpack/blob/main/AGENTS.md). That cross-repository link is for consultation and provenance; agents must not assume its contents are automatically loaded.

Guiding principle: develop convincing, systemic horror through world interaction and player agency, rather than relying only on jump scares or scripted effects. Keep costly AI/world simulation bounded and avoid irreversible destruction without explicit design approval.

This project is **independent** of Too Much Magic. Do not assume its technical version, mod roster, branch layout, or release/QA permissions carry over.
