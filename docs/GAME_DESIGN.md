# Game Design

## Product vision

Turfer is a single-player operational RTS played on a large topographic map. The player manages a state, several military formations, and the infrastructure that keeps them operating. The central decisions are where to build, which routes to protect, which objectives to attack, when to hold, and when to retreat.

The game should reward controlling a city, port, bridge, road junction, valley, or mountain pass even when the player owns less total land. Rapid expansion without food, fuel, ammunition, and a secure route should stall and may collapse.

Target match lengths:

- Short scenario: 15 to 25 minutes.
- Standard match: 30 to 50 minutes.
- Large scenario: 60 to 90 minutes.

The game supports pause and 0.5x, 1x, 2x, and 4x simulation speed.

## Player experience principles

- The map is the primary interface.
- Every building solves a specific economic or military problem.
- Every unit has a distinct role, weakness, and logistics cost.
- Distance, terrain, elevation, roads, rivers, and bridges affect all operations.
- A player can understand why a unit slowed, weakened, retreated, or lost.
- A player is not required to occupy worthless land.
- Logistics is visible and strategically important without becoming truck micromanagement.
- Losing a supply route creates a recoverable crisis, not an instant unexplained defeat.

## Visual direction

The visual style is a clean modern military map in a top-down 2D view. Use hillshade, subtle elevation colors, and light contour lines to create depth. Avoid both noisy realism and a childish style.

Territory color should use roughly 35 to 45 percent opacity so terrain remains readable. Borders should be stronger than territory fill. Neutral land must look intentional rather than unfinished.

### Level of detail

- Far zoom: borders, fronts, major cities, ports, and large formations.
- Medium zoom: roads, factories, supply routes, and unit composition.
- Close zoom: movement direction, local supply state, damage, construction, and restrained vehicle animation.

Icons must have distinct silhouettes. Ownership is shown by an outline or ring, not by replacing the icon's identity.

## Map model

Each map cell stores at least:

- Elevation
- Slope
- Terrain type
- Passability by unit category
- Base logistics capacity
- Defensive modifier
- Visibility modifier
- Economic suitability
- Road, bridge, river, coast, or water state
- Ownership eligibility

### Strategic terrain

Cities, ports, factories, road junctions, bridges, passes, valleys, farmland, and industrial sites usually justify control.

### Traversable low-value terrain

Open plains, distant forests, empty hills, and deserts can matter for connection, flanking, defense in depth, or encirclement. They do not provide victory points merely because they are colored.

### Hostile or unusable terrain

Peaks, cliffs, glaciers, deep water, extreme swamp, and disconnected wasteland may be restricted or impassable. Some cells can never receive ownership. Other cells are traversable at high time and supply cost but produce no value.

Borders should follow natural features. Automatic hole filling must never claim a mountain, lake, or inaccessible region. It may close only a small, reachable, geographically sensible pocket.

## Terrain behavior

The initial values below are tuning defaults. Store them in validated configuration rather than gameplay code.

| Terrain | Infantry speed | Armor speed | Defense | Main effect |
| --- | ---: | ---: | ---: | --- |
| Plain | 100% | 100% | Normal | Fast movement and easy construction |
| Forest | 85% | 65% | +25% | Concealment and infantry advantage |
| Hills | 75% | 60% | +35% | Elevation advantage and slower travel |
| Mountains | 50% | Road required | +50% | Natural barrier and choke point |
| Desert | 85% | 90% | Normal | Increased supply consumption |
| Swamp | 55% | 35% | +10% | Severe movement obstacle |
| Urban | 75% | 55% | +40% | Slow combat and strong defense |
| Water | Blocked | Blocked | N/A | Ships only |

Uphill movement is slower. Downhill movement receives only a small capped benefit. A higher defender receives up to a 20 percent advantage. Armor cannot cross an extreme slope without a road. Infantry can cross selected mountain terrain at a substantial time and food cost. Peaks and cliffs marked impassable remain neutral.

A mountain pass is a strategic entity separate from the surrounding peaks. The pass, valley, or road can be occupied while the mountains remain neutral.

## Rivers bridges and coasts

River crossing without a bridge is allowed only at suitable points and applies a movement and combat penalty. A road crossing creates a bridge entity that can be damaged, disabled, and repaired. Destroying a bridge does not delete the connected roads.

The first version allows unit loading and unloading through ports only. It does not support unrestricted landing on every coast.

## Fog of war

Terrain is known in advance, but enemy units and the current state of enemy buildings require observation. Elevation improves vision. Forest and urban terrain reduce detection.

When contact is lost, show the last-known position and the age of that information. Do not continue updating the hidden unit. Large buildings remain marked after discovery, but their current damage and production state become unknown.

Bots consume the same information. They may remember observations and infer likely movement, but they cannot query exact hidden positions or inventories.

## Resources

The main resources are:

- Money
- Manpower
- Food
- Fuel
- Weapons and ammunition

Money is a state-level abstraction. Manpower, food, fuel, and weapons originate at locations and move through the logistics network according to demand.

Cities produce money and manpower. Food factories produce food. Fuel factories produce fuel. Weapons factories produce equipment and ammunition. Roads and ports determine distribution capacity.

Players set high, normal, or low supply priority for a front, formation, or city. Distribution is automatic. The UI always shows the current source, limiting route segment, and estimated reserve time.

## Supply network

Supply moves on a graph connecting cities, factories, roads, ports, and front destinations. Every segment has capacity. Basic roads, improved roads, mountain routes, off-road movement, and sea lanes have different throughput.

Units carry a local reserve. A full reserve should support roughly one minute of ordinary activity, with unit-specific adjustments. Armor burns fuel faster during intensive operations. Infantry can continue longer without fuel.

Supply states:

- Green: above 60 percent.
- Yellow: 20 to 60 percent.
- Red: below 20 percent.
- Empty: the internal reserve is exhausted.

Without food, reinforcement stops and morale declines. Without fuel, vehicles lose most movement capability and cannot begin a major offensive maneuver. Without ammunition, fire rate and damage decline toward a minimal defensive capability.

A small amount of supply may cross open terrain so the system is not completely binary, but roads provide a decisive throughput advantage.

Convoys are generated and routed automatically. They are visible and vulnerable. Destroying a convoy loses its cargo only. The network creates another convoy when stock, capacity, and a route are available.

## Buildings

### City

A city produces money and manpower, stores resources, reinforces and repairs units, and carries victory value. Cities are pre-placed in the first version and can be upgraded but not freely founded.

Capturing a city requires continuous local control. Active defenders slow capture. A captured city enters a temporary reorganization state with reduced output. Some inventory survives, some is lost, and some is destroyed according to configuration.

### Road

A road increases movement speed and supply capacity. Initial tuning targets are roughly a 40 percent movement increase for a basic road and up to 80 percent for an improved road.

A disconnected road still exists but cannot carry network supply. A damaged road provides a partial benefit and can be repaired.

### Food factory

A food factory receives a terrain suitability bonus on fertile plains, near rivers, or in agricultural zones. It consumes money and manpower. When disconnected, it continues only while local inputs and storage permit.

### Fuel factory

A fuel factory receives a suitability bonus in designated energy regions. The first version does not require a separate oil-well entity. Damage forces the state to use reserves instead of instantly stopping every vehicle.

### Weapons factory

A weapons factory consumes money, manpower, and a configured amount of fuel. It supplies new-unit equipment and ammunition. It performs best near a large city or industrial zone.

### Port

A port requires a valid coast and access to open water. It must connect to the land network by road to reach full capacity. Friendly ports create sea supply routes. Loading, unloading, and convoy departure take visible time.

## Construction damage and capture

Construction requires money, manpower, time, suitable terrain, and connection to a friendly center. The placement preview shows cost, duration, suitability, expected connection, and estimated output. An invalid placement shows the exact reason.

Damage reduces output gradually. Repair consumes money and time and has a configurable priority. A captured building remains damaged and enters reorganization; the attacker never receives immediate full output.

## Initial unit roster

### Infantry

The primary capture and holding unit. It uses little fuel, performs well in forest, urban, and selected mountain terrain, and is vulnerable to armor and artillery in open terrain.

### Armor

The breakthrough and exploitation unit. It is fast on plains and roads, consumes substantial fuel and ammunition, and performs poorly in urban, mountain, and swamp terrain. Fuel loss sharply reduces its operational value.

### Artillery

A ranged support unit. It cannot capture territory, consumes ammunition quickly, and is vulnerable at close range. It requires friendly observation of the target.

### Supply convoy

An automatically generated logistics unit. It moves cargo between network nodes and front destinations. It is vulnerable to raids but does not require routine manual control.

### Transport ship

A port-to-port unit and supply carrier. Loading and unloading take time. It consumes fuel and cannot begin or end a journey without a valid port in the first version.

Every formation has current and maximum strength, morale, cohesion, food, fuel where applicable, ammunition, speed, vision, active order, and current supply source.

New formations appear at cities or defined recruitment centers. Recruitment consumes manpower, weapons, and initial food. Reinforcement uses the same resources and takes time.

## Orders and controls

Default controls use left click to select and right click to issue a contextual order. Drag selects multiple formations. Shift queues orders.

Required orders:

- Move
- Attack
- Attack-move
- Hold
- Retreat to the nearest valid rear position
- Support a formation or front
- Set supply priority
- Prefer the fastest or safest route

Before confirmation, show the planned path. Slow, blocked, dangerous, and unsupplied segments use different styling. Estimated arrival time includes terrain, road, and fuel state.

Group orders preserve readable spacing without requiring detailed formation micromanagement.

## Match flow

### Scenario selection and start

The player chooses a map, bot count, difficulty, and scenario. An open-map mode may offer balanced starting cities. Before confirmation, show terrain, roads, agricultural and industrial suitability, nearby ports, and enemy distance.

### Establishment

The player begins with one city, a small infantry force, limited reserves, and a short road connection. The player takes nearby objectives and chooses the first factory. Bots must allow enough time for a first economic decision before launching a serious attack.

### Network creation

The player connects cities, factories, ports, and fronts. Terrain and objective placement should make expansion, military pressure, and economic security compete for attention.

### Active front

Infantry holds ground, armor searches for a breakthrough, artillery supports, and convoys sustain operations. The player chooses the attack axis, participating forces, reserve, and supply priority.

### Breakthrough encirclement and retreat

Armor can exploit a road after a breakthrough but risks losing its route. An encircled force consumes its reserve, loses morale, and may break out, hold, or retreat. Retreat is a first-class order, not an error state.

### Resolution

The match ends when scenario objectives are complete or a side has lost the practical ability to continue. Empty mountains, remote wasteland, and irrelevant isolated units do not need to be captured.

## Combat and capture

Combat considers unit type, current strength, morale, cohesion, supply, terrain, elevation, entrenchment, and artillery support. The UI lists every material modifier.

Only capture-capable units create control. Artillery never captures by itself. Ownership advances from the front instead of recoloring a large region instantly. The system should prevent tiny noisy ownership islands without filling impassable terrain.

Cities, ports, and factories require continuous local control before ownership changes. Returning defenders stop or reverse capture progress.

## Victory and scenario objectives

The default control victory requires holding the required capitals and a majority of city victory points for 180 seconds, with active logistics connection to at least part of the objective network.

Decisive victory occurs when every bot has lost its capital, production capability, and practical ability to recover a supply center. The player is not required to hunt a lone unit in remote terrain.

Alternative scenarios can require holding a mountain pass, defending a city, taking a port, cutting off an army, destroying fuel production, or evacuating units.

## User interface

The top bar shows money and rate of change, manpower, food, fuel, weapons, ammunition, estimated reserve time, and scenario progress.

The unit panel shows strength, morale, supplies, supply source, arrival time, terrain effects, active order, and path. The building panel shows output, local inventory, network connection, missing inputs, damage, repairs, and destination of production.

Alerts appear only when a decision is needed: a city under capture, a disconnected front, a stopped factory, a projected shortage, a destroyed critical bridge, a blocked port, or an encircled formation.

All UI strings use the localization system. The interface should support English and Hebrew, including RTL, from the beginning.

## Smooth movement and camera

The simulation uses a fixed timestep. Rendering runs independently through the normal render loop. Logical and visual positions are separate.

Each moving entity has a motion plan containing a path, start tick, segment durations, speed, direction, and plan identifier. Extend OpenFront's motion-plan approach rather than emitting a new visual position every tick.

The renderer interpolates along the path. It does not snap a sprite to each cell center. Rotation follows the shortest arc. A changed route begins from the current visual position. A small correction blends over roughly 200 to 400 milliseconds. Immediate snapping is reserved for creation, deletion, map load, intentional teleport, or a severe invalid state.

Road entry, road exit, turns, and speed changes use short visual acceleration and deceleration. Simulation timing remains authoritative.

The camera pans by drag and WASD. Zoom centers on the pointer. Camera easing is restrained and stops predictably. UI interaction never pans the map.

## Bot behavior

Bots evaluate targets by strategic value, risk, distance, terrain, supply capacity, and observed enemy state. They should recognize cities, ports, factories, bridges, junctions, and passes; build useful roads; hold reserves; protect the rear; attack supply routes; retreat from encirclement; and bypass obstacles.

Bots do not attack peaks, build disconnected factories, send armor into impossible slopes, or chase worthless territory while a key city is threatened.

Personality is separate from difficulty. Personalities may be defensive, industrial, aggressive, logistics-focused, or opportunistic. Difficulty changes decision cadence, threat evaluation, and planning depth, not free resources, vision, damage, or speed.

Strategic AI does not run every tick. Pathfinding, economic planning, and target evaluation use separate schedules and caches. AI state is included in saves.

## Performance targets

Initial desktop target:

- 60 FPS at 1080p with 500 operational formations and thousands of map entities on a mid-range desktop.
- At least 30 FPS under exceptional load.
- Normal frame time below 16.7 ms at p95.
- No complete supply-graph or pathfinding recomputation every render frame.
- Reduced rendering detail for off-screen entities while simulation continues.

If the existing architecture cannot meet a target, measure the bottleneck and propose an evidence-based revision. Do not hide poor performance by disabling movement interpolation.

