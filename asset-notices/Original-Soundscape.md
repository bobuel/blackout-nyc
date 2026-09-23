# Original local soundscape foundation

`tools/art/build_soundscape.py` is the editable source for 26 new WAV assets under `assets/audio/original`. It uses Python and NumPy, deterministic seeds, filtered noise, damped resonances, additive instrument synthesis, and short room reflections. No hosted model, account, purchased sample, borrowed melody, or external recording is involved in these files. The original WAVs, source, asset hashes, and measured levels are included.

The existing Kenney contact sounds and CC0 wind/fire recordings remain unchanged and retain their separate notices. No dialogue or warning has a recorded human voice in this pass. Written information remains essential.

## Runtime behavior

- Three door, cloth, and handgun variations avoid immediate repetition. Revolver handling and opening a dressing now have distinct sounds.
- Four continuous city-air beds crossfade with elapsed campaign time: night, dawn, day, evening. They complement the existing wind instead of replacing positional fire.
- Occasional shutter, metal, engine, and rolling-work-train textures differ by neighborhood. They represent incidental off-screen activity and convey no quest facts or unannounced alarm.
- Six original sparse keyboard phrases last about 26–35 seconds, followed by 48–110 seconds of silence. Neither of the previous two phrases can play next. Music is not a short looping track.
- Tactical focus lowers music and ambient beds. It does not add a victory fanfare or assign an emotional score to a moral choice.
- Soundscape scheduling uses a separate random generator. It cannot change encounter variation, campaign time, or saved consequences.
- The existing player controller owns concrete footsteps and punch contact. The soundscape adds occasional quiet clothing movement without duplicating footsteps.

`soundscape.bind(player, tactics)` retains the existing integration. `set_campaign_context(chapter_id, elapsed_minutes)` should be called when campaign time changes. `route_scene_audio(world, player, actors)` places existing wind/fire on Ambience and actor footsteps/contact on Effects after spawning. `set_mix_levels(music_db, ambience_db, effects_db)` changes those buses once, while the existing master control remains authoritative. Values of -40 dB or below explicitly mute the category; moving above that minimum unmutes it. `play_at(key, location, volume)` retains the door callback and can play other named local cues. Every file is available offline.

## Render and verification

Run `tools/art/build_soundscape.py`, then `tests/audio/check_rendered_audio.py`. The second script reads the actual PCM files, checks content hashes, sample format, DC offset, peak and RMS levels, silent transient endpoints, continuous loop boundaries, and clipping. Source peaks remain below 0.65; the conservative 68-second review mix peaks around 0.148. Runtime buses add limiters and bound simultaneous sound voices.

The generation command writes `artifacts/audio/soundscape_audition.wav` with a cue sheet and a machine-readable PCM report. This is a review render, not a capture of finished gameplay. `tests/audio/test_soundscape.gd` checks Godot loading, time crossfades, signal response, rejected actions, finite music scheduling, rebind cleanup, category routing, and actual mute controls. `tests/audio/render_runtime_mix.gd` records a separate 21-second mix from Godot's own audio server to `artifacts/audio/godot_runtime_mix.wav`, including positional effects and changing context.

The audio-input tool for this model reports that listening input is unsupported. Rendered audio and measured levels are verified; no subjective listening acceptance is claimed. Real hardware playback, perceived balance alongside dialogue/subtitles, sound-to-animation contact, full-run fatigue, and final emotional quality remain review gates. Enemy gunshots use explicit opponent-action events and the acting opponent's position.

