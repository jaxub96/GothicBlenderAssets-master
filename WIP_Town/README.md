# GothicTown.blend — work in progress

A two-floor Gothic-style house (`NW_TOWN_HOUSE_01`) built with the
`gothic-terrain-piece` skill's building-kit functions (see the
`Blend-Zen` repo's `.claude/skills/gothic-terrain-piece/`), started as a
simple single-room cottage and iterated up into a fuller building. Built
entirely via the blender MCP tools (`execute_blender_code`), not modeled
by hand.

## What's in the scene

- Ground floor: 2 rooms (hall + back room) split by an interior partition
  with its own doorway, a door (south wall), a window (east wall).
- A straight staircase (10 steps) from the ground floor up through an
  opening in the floor slab.
- Second floor: its own door (south, opens onto a balcony) and window
  (north), sitting directly on the ground floor's wall-top ring.
- A cantilevered balcony (platform + 3-sided parapet) off the second
  floor's south door.
- Gable roof with eave overhang over the second floor.
- An independent single-story annex (west of the main house, one grid
  cell of clearance from it) with its own door and a lean-to roof.
- Materials: 5 distinct stone wall texture variants (different per floor
  and per building), so it doesn't read as one texture tiled everywhere;
  uniform cube-projection UVs across the whole model.

## Known issues (not yet fully resolved)

Verified via script (boundary-edge audit + connectivity flood-fill), not
just visually:

- **The staircase is topologically a separate piece** from the rest of
  the mesh -- it sits flush on the floor but its base vertices aren't
  welded to the floor grid. Not visually obvious, but "Select Linked"
  from it won't reach the house.
- **~63 non-rim boundary edges remain** (down from 134 at the start of
  the fix pass) -- mostly around door jambs where the adjacent-segment
  T-junction fix didn't fully propagate everywhere it should have, a few
  around the annex roof's overhang boundary, and 2 at the ridge peaks
  (`ridge[i0]`/`ridge[i1]`) that were never fully diagnosed. Re-run the
  boundary-edge check (see `SKILL.md`'s "Lessons from building a
  multi-room house" section for the exact script) before trusting this
  mesh is watertight.
- No chimney, quoins, or foundation plinth yet (scoped out for time).
- The annex has its own exterior door but is not interior-connected to
  the main house.

## Continuing this work

Read `Blend-Zen/.claude/skills/gothic-terrain-piece/SKILL.md` in full
before touching this file again, especially the "Lessons from building a
multi-room house" section at the end -- it documents several
easy-to-reintroduce bugs (T-junctions, unclosed thin-wall bases, parapet
corner gaps, z-fighting between adjacent buildings) that this file hit
and fixed once already.
