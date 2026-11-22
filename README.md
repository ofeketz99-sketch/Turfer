# Turfer

Turfer is a planned single-player operational real-time strategy game built on the open-source foundations of [OpenFront](https://github.com/openfrontio/OpenFrontIO).

The game is about controlling strategic objectives, terrain, infrastructure, and supply networks. It is not about painting every map tile. Mountains, cliffs, deep water, remote deserts, and other low-value areas may remain neutral for the entire match.

## Project status

Design and pre-implementation. No OpenFront source code has been imported yet.

## Start here

AI coding agents and human contributors should read these files in order:

1. [`AGENTS.md`](AGENTS.md) — mandatory repository working rules.
2. [`docs/DESIGN_DECISIONS.md`](docs/DESIGN_DECISIONS.md) — locked product decisions and scope boundaries.
3. [`docs/GAME_DESIGN.md`](docs/GAME_DESIGN.md) — the authoritative game design.
4. [`docs/IMPLEMENTATION_PLAN.md`](docs/IMPLEMENTATION_PLAN.md) — implementation sequence and phase gates.
5. [`docs/ACCEPTANCE_CRITERIA.md`](docs/ACCEPTANCE_CRITERIA.md) — testable completion criteria.

When documents conflict, use the order above. `DESIGN_DECISIONS.md` overrides older or more general design text.

## Core product definition

- Single-player only, against one or more bots.
- Local deterministic simulation with pause, save/load, replay support where practical, and 0.5x, 1x, 2x, and 4x speed.
- Terrain affects movement, combat, vision, construction, and logistics.
- Cities, roads, food factories, fuel factories, weapons factories, and ports each have a distinct role.
- Food, fuel, and weapons move through a visible supply network.
- Victory comes from scenario objectives and strategic locations, not total map coverage.
- Rendering must interpolate movement smoothly between simulation ticks.
- Bots follow the same information and resource rules as the player.

## OpenFront and licensing

Before importing or modifying OpenFront code:

- Record the exact upstream commit used as the base.
- Review `LICENSE`, `LICENSE-ASSETS`, copyright notices, and attribution requirements.
- Preserve all notices required by the GNU AGPL and by individual asset licenses.
- Do not assume every visual asset is licensed under the same terms as the source code.

## Current repository contents

This repository currently contains product and implementation guidance only. The first engineering task is the repository and upstream audit described in `docs/IMPLEMENTATION_PLAN.md`.

