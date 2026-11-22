# Implementation Plan

The project must progress through complete vertical slices. Do not implement every data model first and postpone the playable loop.

## Phase 0 Repository and upstream audit

### Work

- Identify the exact OpenFront commit to use.
- Run the upstream game and its complete automated test suite unchanged.
- Record package boundaries, local-game flow, rendering, pathfinding, map loading, motion plans, save/replay support, and server dependencies.
- Review `LICENSE`, `LICENSE-ASSETS`, copyright notices, and attribution requirements.
- Measure baseline startup time, simulation cost, render performance, and memory on a representative map.
- Write the results to `docs/UPSTREAM_AUDIT.md`.

### Gate

- The upstream commit is pinned.
- Required notices and asset constraints are documented.
- Existing tests pass or every baseline failure is recorded.
- The proposed reuse and replacement boundaries are evidence-based.

## Phase 1 Playable vertical slice

Build one 10 to 15 minute scenario containing:

- Two cities
- One impassable ridge and one mountain pass
- One river and bridge
- Two road routes
- One food factory, fuel factory, and weapons factory per side
- One port per side
- Infantry and armor
- One local supply network
- One bot
- Save/load
- One complete objective-based win condition

Use placeholder art when necessary. The goal is to prove the loop, not content breadth.

### Gate

- The scenario can be played from start to victory without debug intervention.
- Neutral impassable terrain remains neutral.
- The bot pursues objectives instead of total map coverage.
- Cutting and restoring a route changes unit operation gradually.
- Save/load restores simulation, AI, supply, and active movement.

## Phase 2 Map terrain and movement

### Work

- Add elevation, slope, terrain, passability, economic suitability, and ownership eligibility to map data.
- Implement terrain-aware route costs.
- Implement motion plans, visual interpolation, smooth turning, road entry/exit, and correction blending.
- Implement camera controls and initial level of detail.
- Verify speed changes and save/load during motion.

### Gate

- Long movement contains no visible tile jumps.
- Armor routes around an impassable ridge and uses a pass or road.
- Infantry can use configured difficult terrain at the correct cost.
- Simulation results remain identical at different render frame rates and game speeds.

## Phase 3 Economy and logistics

### Work

- Implement city output and storage.
- Implement the three factory types.
- Implement road and bridge capacity and damage.
- Implement ports and sea connections.
- Implement local inventories, supply demand, priorities, and automatic convoys.
- Add supply overlays and limiting-segment explanations.

### Gate

- Every resource can be traced from origin to consumer.
- Disconnection drains local reserves before applying penalties.
- Reconnection restores operation gradually.
- Save/load cannot duplicate or delete inventory.
- The player never needs to command individual convoys for ordinary operation.

## Phase 4 Combat capture and retreat

### Work

- Implement strength, morale, cohesion, ammunition, fuel, and food effects.
- Implement terrain, elevation, river, bridge, and urban modifiers.
- Add artillery support and observation requirements.
- Implement gradual control, capture, building reorganization, reinforcement, encirclement, and retreat.

### Gate

- Every material combat modifier appears in the UI.
- Artillery cannot capture alone.
- A surrounded force degrades gradually and can retreat or break out when a valid route exists.
- Ownership does not fill impassable or ineligible terrain.

## Phase 5 Information and interface

### Work

- Implement fog of war and last-known information.
- Add terrain, passability, supply, range, suitability, and objective overlays.
- Add unit and building panels.
- Add actionable alerts and invalid-action explanations.
- Verify English and Hebrew layout, including RTL.

### Gate

- A new player can explain roads, factories, unit stoppage, supply source, objective value, and victory requirements within ten minutes.
- Bots cannot access hidden state.
- The UI identifies the exact reason for an unavailable build or order.

## Phase 6 Bot expansion balance and scale

### Work

- Add bot personalities separately from difficulty.
- Run automated bot-versus-bot simulations for economy, routing, and stalemate detection.
- Tune balance through configuration and measured scenarios.
- Test larger maps and optimize pathfinding, supply, rendering, and AI schedules.

### Gate

- Each personality produces observably different priorities without cheating.
- Bots protect objectives and supply before irrelevant land.
- Performance targets pass or measured exceptions are documented.
- The standard match length remains within the target range.

## Reporting after every phase

Report:

- Files changed
- User-visible behavior completed
- Automated and manual tests run
- Performance measurements
- Known limitations
- Documentation updated
- The next smallest safe step

Visual changes require a screenshot or short recording from the running build.

