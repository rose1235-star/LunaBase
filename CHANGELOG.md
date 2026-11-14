# Changelog
## v2 (this version, untested in engine)
- Habitat rebuilt as walkable Hab Core + airlock chamber; old solid airlock box and "shelter ring" removed (resupply and shielding now happen inside the structure). Reason: a functional airlock needs a real interior.
- NEW: airlock state machine (hatches, interlocks, pressure, suit check, panels, emergency entry).
- NEW: crew NPCs, shared astronaut builder, dialogue and one choice.
- NEW: comms system, radio panel, waveform, subtitles, history.
- NEW: habitat systems (battery, solar, oxygen, water, temperature) and scripted events.
- NEW: suit temperature and integrity; "thermal" failure.
- NEW: audio manager and 20 synthesised sounds; settings panel.
- NEW: first-person camera (auto indoors, V to toggle) with blended transitions.
- Sky: denser stars, smaller Earth with a phase, sun disc, darker ambient.
- Terrain: 40 small craters added, faster height lookup.
- HUD rewritten (all v1 meters kept: oxygen, water, power, radiation, samples, objective, warnings, prompts, end screen).
- Preserved from v1: movement/jump/run, lunar gravity, 4 samples, survival drain/refill logic, success/failure screens, restart (R), F2 shadows.
## v1
- Original playable mission (see v1 ZIP).

## v3 (untested in engine)
- Terrain: procedural regolith material (grain colour + pebble bump map, world-space triplanar). Only terrain.gd changed; revert by copying terrain.gd from v2.

## v4 step 1 (untested in engine)
- NEW scripts/mats.gd: cached material library with generated panel/plate textures and normal maps (world-space triplanar).
- NEW scripts/habitat_detail.gd: batched interior detail, equipment, signage, live wall display, emergency lighting.
- habitat.gd: uses Mats materials, creates the detail node, forwards update_visuals; orange band on the back wall no longer coplanar with the wall face (likely z-fighting).
- Backup of the previous state: LunaBase_v3_backup_before_v4 (separate folder).
