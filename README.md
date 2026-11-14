# LunaBase v2 – Junior Astronaut Mission Trainer (Godot 4.7.2, GDScript)

## Honest status (please read)
**v2 has NOT been run in Godot.** I have no Godot engine in my working environment, so nothing below was launched, played or listened to. What I did verify by script: every `res://` path and sound file exists, the main scene is set, brackets and indentation are consistent, there are no tabs and no duplicate functions, and every cross-script function/variable call matches a definition. The v1 project you already have is untouched (a copy is in `LunaBase_v1_backup` inside the folder I used; your own v1 folder/ZIP is the real backup, so **do not delete it**). If Godot shows a red error, send it to me and I will fix it.

## Open and run (beginner steps)
1. Keep your v1 project where it is. Unzip `LunaBase_Godot_v2.zip` to a NEW folder, e.g. `C:\LunaBase_v2` (about 1.5 MB of audio; nothing else to download).
2. Open Godot 4.7.2. In the Project Manager click **Import**, choose `project.godot` in that folder, then **Import & Edit**.
3. Wait for the first import (Godot converts the .wav files; a few seconds).
4. Press **F5**. The main scene is already set. Renderer is Compatibility.
5. Read the briefing and press **Enter**.

## Controls
WASD/arrows move, **Shift** run, **Space** hop, **Mouse** look (Esc frees the cursor), **E** interact, **V** first/third person (first person is automatic indoors), **1 / 2** answer questions, **H** communication log, **O** audio settings (mouse needed), **F2** sun shadows, **R** restart.

## The mission (full loop)
1. Talk to **Commander Reyes** (E). Answer "Ready" to receive EVA authorization.
2. Walk into the **airlock chamber** (through the inner hatch). Use the wall panels (E) to **close the inner hatch** and **run the suit check** (suit must be topped up: resupply happens automatically inside the powered habitat).
3. **Depressurize** the chamber (panel on the right wall). Watch the pressure fall. **Open the outer hatch** and step out.
4. Follow the **blue beams** to 4 samples. Events happen (see below). Watch the HUD.
5. Return. Enter the chamber, **close the outer hatch**, **repressurize**, **open the inner hatch**, walk in, and press E at the **science bench** to deliver the samples. Debrief screen.
Interlocks are real rules in the code: both hatches can never be open, the outer hatch will not open until the chamber is at vacuum, the inner will not open until it is pressurized, and a warning explains every refusal. If you are locked outside, the exterior panel can start an emergency depressurize-and-open.

## Scripted events (not random, each fires once)
- After sample 1: **solar array fault** (habitat on battery). Reset the breaker on the power console, or it recovers by itself after 90 s. If the battery empties: blackout, airlock on slow manual mode, no suit resupply.
- After sample 2: **solar particle event** (radiation rate x2.5 for 45 s while outside).
- Sample 3: **suit snag** (integrity -22%, oxygen leaks until repaired in the habitat).
- Over 75 m from the habitat: **radio blackout** for 14 s (messages are held, then delivered).

## What is implemented in v2 (by script checks only; untested in engine)
| Area | What exists |
|---|---|
| Environment | ~1600 stars (varied size/brightness, a denser band), Earth ~2 degrees wide lit by the sun so it shows a phase, sun disc, black sky, lower ambient light, 64 craters (small + large), boulders with collision, blob shadows under characters |
| Astronauts | Shared builder; 3 NPC crew with different suits, idle/work/patrol animation, face the player, gestures, talk (E). The 4th crew member (EVA astronaut) is you |
| Airlock | Walkable chamber, two sliding hatches with collision, interlocks, pressure simulation (kPa), suit check, 5 control panels, lamps, in-world readouts, manual mode, emergency entry |
| Habitat | Walkable Hab Core with consoles, bench, locker, lights; exterior modules, tilting solar arrays, dish, tanks, rover |
| Survival | Suit: oxygen, water, power, temperature, integrity, radiation. Habitat: battery, solar, oxygen, water, temperature. Failure causes: oxygen, power, radiation, overheating |
| Comms | Event-driven queue with priorities, mission control + crew, radio vs face-to-face, subtitles, animated waveform, history (H), one dialogue choice, blackout |
| HUD | Suit telemetry, breath indicator, habitat + crew panel, airlock panel, comms panel, flashing alerts, objective, timer, camera mode, briefing and debrief |
| Audio | 20 synthesised sounds, audio buses, settings panel (volumes, mute, subtitles), vacuum rules |
| Camera | Third person with ground clearance, first person with blended transitions |

## Audio asset inventory (`audio/`, all synthesised by `tools/make_audio.py`, original, no licences)
Loops: `breath_loop`, `hab_ambience`, `radio_static`, `airlock_pump`. One-shots: `radio_on`, `radio_off`, `beep_alert`, `alarm_oxygen`, `alarm_radiation`, `alarm_suit`, `power_fail`, `power_restore`, `hatch_open`, `hatch_close`, `pressure_hiss`, `footstep`, `footstep_contact`, `sample`, `suit_check`, `ui_click`.
These are simple generated sounds, not recorded or studio-quality. They only play if Godot imports them; the options panel (O) tells you if any file is missing. I have not heard them. There is **no spoken voice acting**: crew speech is subtitles plus the radio sounds.
Vacuum rule: habitat ambience, hatches and the pump are heard only while you are in air. Outside you hear suit breathing, helmet radio and footsteps as low contact vibration.

## Known limitations
- Untested in the engine (see above); expect small fixes.
- Interior shares the same sun light (no true indoor/outdoor light separation); sun shadows are off by default for the Celeron (F2).
- Crew are simple primitives, not rigged models; no lip-sync; no voice lines.
- The player model was not changed (v1 model kept); crew use a new shared builder.
- Habitat exterior modules (rear module, dome) are not enterable.
- All numbers (rates, kPa cycle time, dose, temperatures, events) are **gameplay assumptions**, not NASA values. Real facts used: lunar gravity 1.62 m/s^2, no atmosphere (no airborne sound, no sky colour), regolith formed by impacts, solar particle events are a real radiation hazard.
- Multiplayer is NOT implemented (see `docs/MULTIPLAYER_ROADMAP.md`).

## Manual test checklist (please run and tell me what fails)
1. F5 launches; briefing appears; Enter starts. 2. WASD, mouse, Space work in the habitat. 3. HUD meters move. 4. E on Commander shows the question; press 1. 5. Crew turn toward you. 6. Inner hatch panel opens/closes the door. 7. Suit check, depressurize: pressure number falls. 8. Opening the outer hatch while pressurized is refused with a warning. 9. Step outside; third-person camera; footstep thumps. 10. Samples collect; events fire. 11. Return and repressurize; deliver at the bench; debrief shows. 12. R restarts. 13. O opens audio options.

## Files
`scenes/main.tscn` (only scene), `scripts/` (15 scripts: main, player, survival, habitat, airlock, habitat_systems, comms, audio, hud, crew_member, astronaut_model, panel, sky, terrain, sample), `audio/`, `tools/make_audio.py`, `docs/`.
