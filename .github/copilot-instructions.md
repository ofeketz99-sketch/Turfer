# Turfer Copilot Instructions

Read `/AGENTS.md` and the linked documents before proposing or writing code.

The essential constraints are:

- Single-player only.
- Objective-based victory, not percentage map painting.
- Neutral, impassable, and low-value terrain is intentional.
- Terrain and route-based logistics must materially affect play.
- Bots use the same rules and information as the player.
- Gameplay simulation is deterministic and separate from visual interpolation.
- Work in tested vertical slices and keep balance data in configuration.

If a task conflicts with these constraints, stop and identify the conflict instead of implementing a silent workaround.

