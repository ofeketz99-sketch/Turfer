# Acceptance Criteria

These criteria convert the game design into release gates. Add regression tests whenever a bug reveals a missing criterion.

## Product scope

- The normal game starts locally without requiring a gameplay server.
- The UI contains no multiplayer, matchmaking, account, ranking, clan, or lobby dependency.
- Pause and 0.5x, 1x, 2x, and 4x speed work during a live match.
- A standard match can end while a significant part of the map remains neutral.

## Determinism persistence and replay

- Two simulations using the same map, seed, configuration, and command stream produce the same state hash after 10,000 ticks.
- Render frame rate does not affect simulation state.
- Changing game speed does not affect the final simulation result.
- Save/load restores units, buildings, inventories, routes, active orders, AI state, capture progress, and motion plans.
- Saving during movement and loading again does not create a visible jump or change arrival time.
- Saving and loading cannot duplicate or remove resources.

## Terrain ownership and pathfinding

- Peaks, cliffs, deep water, and other ownership-ineligible cells remain neutral.
- Armor routes around an impassable ridge or uses a valid road or pass.
- Infantry can cross configured difficult terrain at the specified movement and supply cost.
- A bot does not repeatedly request a route to an unreachable cell.
- A bot values a mountain pass above the surrounding peaks.
- River crossing without a bridge is slower and more dangerous than a valid bridge crossing.
- Automatic pocket filling never claims an inaccessible mountain, lake, or equivalent region.

## Buildings and construction

- City, road, food factory, fuel factory, weapons factory, and port icons are distinguishable without text.
- Every building panel states its purpose, inputs, output, local inventory, network state, damage, and current limitation.
- Invalid construction previews show the exact reason and required correction.
- A damaged building loses output gradually.
- A captured building enters reorganization and does not provide immediate full output.
- Ports can be built only on a valid coast connected to open water.

## Logistics

- Destroying or disabling a bridge changes the calculated supply route.
- A disconnected factory continues only until local inputs or storage prevent further output.
- A disconnected unit consumes its local reserve before penalties fully apply.
- Food loss stops reinforcement and reduces morale gradually.
- Fuel loss sharply reduces vehicle movement and offensive capability.
- Ammunition loss reduces fire rate and damage toward a minimal defensive state.
- Reconnection restores stock and capability gradually.
- A port can connect two land logistics networks through a sea route.
- Convoys are generated automatically, carry finite cargo, and can be destroyed without deleting the national stockpile.
- The UI identifies the source and limiting route segment for each supplied formation.

## Movement and camera

- A unit moving over 100 cells does not visibly jump between tiles.
- A changed route does not visually move the unit backward.
- A stationary unit does not jitter.
- Turning uses the shortest visual rotation.
- Road entry, road exit, acceleration, and deceleration are visually continuous.
- Movement remains smooth at 30, 60, and 120 FPS.
- Territory borders do not flicker while control is contested.
- Zoom centers on the pointer and UI interaction does not pan the map.

## Combat capture and retreat

- Combat includes unit type, strength, morale, cohesion, supply, terrain, elevation, entrenchment, and support where applicable.
- The UI lists every material modifier applied to a selected combat.
- Artillery cannot capture territory by itself.
- A city, factory, or port changes owner only after continuous local control.
- Returning defenders stop or reverse capture progress according to configuration.
- An encircled unit consumes reserves and loses capability gradually instead of disappearing immediately.
- Retreat selects a valid rear route, avoids known enemies where practical, and does not break formation movement.
- Reinforcement consumes manpower, weapons, food, and time.

## Fog of war and AI fairness

- Hidden enemy units do not update on the player's map after contact is lost.
- Last-known information includes its age.
- Bots cannot query exact hidden positions, inventories, or orders.
- Difficulty does not grant free resources, damage, movement speed, production, or vision.
- Bots defend a threatened city before pursuing irrelevant empty land.
- Bots do not build a disconnected factory without a plan to connect it.
- Bots do not send armor into impassable mountain terrain.
- Bots retreat from an unfavorable encirclement when a reasonable route exists.

## Victory and scenarios

- Default victory does not depend on total owned map percentage.
- The control victory requires the configured capitals and city victory points for the configured hold time.
- Decisive victory can end the match after the enemy loses capital, production, and practical recovery capability.
- The player is not required to hunt irrelevant isolated units in remote terrain.
- Alternative scenarios can complete through defense, pass control, port capture, encirclement, infrastructure destruction, or evacuation.

## Usability

A new player can determine within ten minutes:

- Why roads matter.
- What each factory produces.
- Why an armor unit stopped or slowed.
- Where a formation receives supply.
- Why a city, bridge, port, or pass matters.
- What is required to win the current scenario.

## Performance

- Target 60 FPS at 1080p with 500 operational formations and thousands of map entities on the agreed reference desktop.
- Maintain at least 30 FPS under exceptional load.
- Keep normal frame time below 16.7 ms at p95 on the reference scenario.
- Do not recompute the complete pathfinding or supply graph every render frame.
- Off-screen entities retain simulation behavior without full rendering cost.
- No recurring garbage-collection pause causes visible movement freezing.

If a target cannot be met, the implementation must include a reproducible benchmark, measured bottleneck, and an approved revised target. Disabling interpolation is not an acceptable performance fix.

## Final vertical-slice gate

The vertical slice is complete only when:

- A player can start, play, save, load, and win the scenario.
- A bot can complete the same loop under the same rules.
- Terrain, supply, buildings, combat, capture, and victory affect the match visibly.
- All relevant automated checks pass.
- The running build has been visually inspected for movement and UI defects.
- Known limitations and next work are documented.

