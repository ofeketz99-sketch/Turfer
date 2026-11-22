# Design Decisions

This file records decisions that are already settled. Treat them as requirements until the user explicitly changes them.

## Decision log

### DEC 001 Single-player only

**Status:** Locked

Turfer is a local single-player game against bots. Multiplayer, matchmaking, online lobbies, accounts, rankings, clans, and networking-driven gameplay are outside the product scope.

OpenFront's deterministic engine remains useful for saves, replays, testing, debugging, and variable game speed. Multiplayer infrastructure should not drive the architecture of the Turfer product.

### DEC 002 Partial map ownership is expected

**Status:** Locked

The map does not need to become fully occupied. Peaks, cliffs, deep water, severe swamps, remote deserts, and other unsuitable terrain can remain neutral from start to finish.

The game must distinguish:

- Strategic terrain that provides an economic, logistical, military, or victory benefit.
- Traversable terrain that matters only for routing, depth, flanking, or connection.
- Hostile or unusable terrain that is expensive, restricted, or impossible to cross and may never receive ownership.

### DEC 003 Objectives determine victory

**Status:** Locked

Default victory is based on cities, capitals, scenario objectives, and the opponent's ability to continue operating. Total owned map percentage is not a default victory condition.

The player must not be forced to capture empty mountains, remote deserts, or isolated surviving units after the enemy has lost its strategic and production capability.

### DEC 004 Terrain is a gameplay system

**Status:** Locked

Elevation, slope, land type, rivers, bridges, coasts, roads, and mountain passes affect movement, combat, visibility, construction, and supply. Terrain is not a decorative background.

### DEC 005 Buildings form one logistics system

**Status:** Locked

Cities, roads, food factories, fuel factories, weapons factories, and ports form one connected economy:

- Cities generate money and manpower, store supply, reinforce units, and carry victory value.
- Roads increase movement speed and supply throughput.
- Food factories support manpower, reinforcement, and morale.
- Fuel factories support armor, transport, ships, and heavy industry.
- Weapons factories create equipment and ammunition.
- Ports connect land networks through sea routes and transfer units.

No building may exist only as a generic percentage bonus.

### DEC 006 Supply is local and gradual

**Status:** Locked

Food, fuel, and weapons move over a capacity-limited network. Units carry a local reserve. A cut route causes gradual degradation rather than instant deletion or immediate total shutdown.

Convoys are visible and vulnerable but are routed automatically. The player controls priorities, not individual trucks.

### DEC 007 Initial unit roster is deliberately small

**Status:** Locked

The first playable version contains:

- Infantry
- Armor
- Artillery
- Automatic supply convoys
- Transport ships

Additional unit classes require a new decision and must not block the vertical slice.

### DEC 008 Fair fog of war

**Status:** Locked

Terrain is known, but enemy units and current building state require observation. The player sees last-known positions after contact is lost. Bots use the same visibility model and cannot read hidden state directly.

### DEC 009 Smooth visual movement

**Status:** Locked

The simulation may use grid positions and fixed ticks. The renderer must use motion plans and interpolation so units, turns, speed changes, borders, and camera movement do not visibly jump.

Save/load and speed changes must preserve motion without visible discontinuity.

### DEC 010 Vertical slice before breadth

**Status:** Locked

Development begins with one small, complete scenario. It must prove terrain, one supply network, buildings, infantry, armor, one bot, save/load, smooth movement, and a complete win condition before the project adds more maps or systems.

### DEC 011 OpenFront licensing must be audited

**Status:** Locked

Record the exact OpenFront commit used as the base. Preserve AGPL obligations, copyright notices, attribution, and asset-specific license requirements. Code and assets must be reviewed separately.

## Explicitly deferred systems

- Multiplayer and online services
- Complex diplomacy and alliances
- Aircraft, missiles, nuclear weapons, and air defense
- Dynamic weather and day/night cycles
- Per-soldier simulation
- Large unit rosters and upgrade trees
- Detailed mining and global trade
- Manual logistics vehicle control
- Free placement of new cities
- Unrestricted amphibious landings

## Open technical questions

These questions must be answered by inspecting the chosen OpenFront commit. They are not permission to change the locked product decisions.

1. Which current OpenFront packages can be reused without retaining server dependencies?
2. Which save and replay components can operate in a fully local game?
3. How should terrain metadata and economic suitability be added to the existing map format?
4. Which existing motion-plan and pathfinding systems are appropriate for land formations?
5. What is the measured unit and map-size performance ceiling on the target hardware?

