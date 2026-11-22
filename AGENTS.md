# Agent Instructions

These instructions apply to the entire repository.

## Read before acting

Read the following files before planning or editing code:

1. `docs/DESIGN_DECISIONS.md`
2. `docs/GAME_DESIGN.md`
3. `docs/IMPLEMENTATION_PLAN.md`
4. `docs/ACCEPTANCE_CRITERIA.md`

Do not rely on an old chat, a DOCX export, or assumptions about OpenFront when the repository documents answer the question.

## Instruction priority

Use this order when requirements conflict:

1. Explicit current user instruction.
2. This `AGENTS.md` file.
3. `docs/DESIGN_DECISIONS.md`.
4. `docs/GAME_DESIGN.md`.
5. `docs/IMPLEMENTATION_PLAN.md` and `docs/ACCEPTANCE_CRITERIA.md`.
6. Existing implementation details.

Do not silently resolve a material conflict. Record the chosen resolution in `docs/DESIGN_DECISIONS.md`.

## Non-negotiable product constraints

- Turfer is single-player only. Do not build or preserve multiplayer product flows unless a later explicit decision changes this.
- The map is not expected to become fully occupied. Impassable and strategically worthless areas may remain neutral.
- Victory is objective-based. Never use total owned map percentage as the default win condition.
- Terrain must affect movement, combat, vision, construction, and supply.
- Supply is local and route-based. Food, fuel, and weapons do not teleport globally.
- Bots use the same rules, vision, resources, and movement constraints as the player.
- Simulation state is deterministic. Rendering is not allowed to change simulation outcomes.
- Movement is rendered smoothly between simulation ticks. Tile-to-tile visual jumping is a release blocker.
- Numeric balance values belong in validated configuration, not scattered magic numbers.

## Mandatory workflow

1. Inspect the current repository and the exact OpenFront base before proposing changes.
2. State the smallest end-to-end slice that proves the requested behavior.
3. Implement through the existing architecture when possible; do not rewrite a working subsystem without evidence.
4. Add or update automated tests for deterministic gameplay changes.
5. Run the relevant tests, type checks, lint checks, and a playable scenario.
6. For visual or movement changes, capture and inspect the actual running game.
7. Report changed files, tests, measured performance, known limitations, and the next safe step.

## Architecture rules

- Keep simulation, rendering, input, and persistence responsibilities separate.
- Use a fixed simulation timestep and a seeded random number generator.
- Do not use wall-clock time or unseeded randomness for gameplay outcomes.
- Player and bot actions must enter the simulation through the same command or intent path.
- Save files must include AI state, active orders, motion plans, local inventories, and supply routing state needed for deterministic restoration.
- Visual interpolation may smooth authoritative movement but may not change collision, capture, combat, or arrival time.
- Expensive pathfinding, supply, and strategic AI work must be cached or scheduled; do not recompute complete graphs every render frame.

## Scope discipline

The first playable version contains infantry, armor, artillery, automatic supply convoys, and transport ships. Do not add aircraft, nuclear weapons, complex diplomacy, weather, a day/night cycle, free-form city founding, detailed raw-material extraction, or manual truck micromanagement before the vertical slice passes its gates.

Prefer deleting premature complexity to designing extension points for hypothetical future systems.

## Documentation discipline

- Update `docs/DESIGN_DECISIONS.md` when a product decision changes.
- Update `docs/GAME_DESIGN.md` when behavior visible to the player changes.
- Update `docs/IMPLEMENTATION_PLAN.md` when phase order or technical strategy changes.
- Update `docs/ACCEPTANCE_CRITERIA.md` with every new user-visible requirement or regression test.
- Keep one authoritative statement for each decision. Link to it instead of duplicating it.

## Definition of done

A feature is not done because it compiles. It is done only when:

- Its full user flow works in the playable build.
- Automated tests cover deterministic logic and important regressions.
- Save/load preserves the feature where applicable.
- Bots handle the feature without hidden information or free resources.
- UI explains material costs, modifiers, failures, and unavailable actions.
- Performance remains within the current budget or a measured exception is documented.
- Relevant documentation is current.

