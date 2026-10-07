# MODLOG - Republic Roll (working title)

CS2 agents ride a skateboard or BMX through real CS2 maps, to Riders Republic's own radio and ride sounds,
with Riders Republic videos on billboards. Solo v1; multiplayer seam kept for later.

## Route (decided 2026-10-07)
- **Standalone program that plays from the player's CS2 copy** (Melty mode `standalone`, CS2 `primary`,
  launch `{managed}/RepublicRoll.exe --game {game}`), same pattern as Melty's live "Spike Rush".
- Never loads into CS2 or Riders Republic: reads their files only. No VAC / BattlEye exposure.
- Riders Republic is not in Melty's catalog, so its recipe role is `secondary`. The program finds it itself through
  Steam's `libraryfolders.vdf` (app 2290180, installdir `RidersRepublic`) and says so on screen when missing.
- Engine: Godot 4.7.2 .NET (C#). CS2 assets converted at first run by Source2Viewer-CLI (VRF, MIT) into a
  cache under `%LOCALAPPDATA%/RepublicRoll/cache`. RR audio decoded by vgmstream; RR video to Theora by FFmpeg (LGPL).

## Installed tools (all in `%LOCALAPPDATA%/RepublicRollTools`, user scope, no admin)
.NET SDK 8.0.425 + 10.0.401 (dotnet-install.ps1), MinGit 2.56.0, Godot 4.7.2 mono + export templates,
vgmstream r2117, wwiser v20260808, FFmpeg n8.1 LGPL (BtbN), Source2Viewer-CLI 20.0, universal-modder (clone),
Python numpy (pip, user). Research scripts and derived data: `%LOCALAPPDATA%/RepublicRollTools/work` (never committed).

## Facts - Counter-Strike 2 (Steam 730, build 25738536)
- `game/csgo/pak01_dir.vpk` (49.6 GB). Maps: `game/csgo/maps/<map>.vpk`, world `maps/<map>/world.vwrld_c`,
  entities `maps/<map>/entities/default_ents.vents_c`.
- `characters/models/**.vmdl_c` are 4.8 KB **stubs** (refMeshes empty). Real agents: `agents/models/<agent>/<agent>.vmdl_c`.
- Agent export needs `--gltf_export_animations` (with a non-matching `--gltf_animation_list`) to keep the skin:
  94 joints, names `pelvis, spine_0..3, neck_0, head_0, arm_upper_L, arm_lower_L, hand_L, leg_upper_L, leg_lower_L, ankle_L`.
- VRF glTF: metres, Y-up. Source (x,y,z) inches to glTF (x, z, -y) * 0.0254 (to verify in engine with spawn raycast).
- Dust2 world export: 16 s, 320 MB glb + 570 PNG (~1 GB). `_physics.glb` holds only entity triggers, not world collision.
- T spawn dust2 example: origin [-822.4, -795.6, 150.7], yaw 107.

## Facts - Riders Republic (Steam 2290180, build 24114024, Anvil engine, BattlEye)
- `.forge` archives hold models and text and are **not readable**; `localization.lang` is 19 bytes (language switch).
  So the "gear names" feature is dropped; billboards caption with event names from video file names instead.
- `videos/*.webm` (136 files) readable as-is.
- `sounddata/pc/*.pck` = Wwise AKPK v1, bank version 135. `sounds_sfx.pck` (100 banks, 2023 streams),
  `sounds_sfx_patch_1_.pck` (64 banks, 271 streams). Banks embed media (DIDX/DATA); streams are in the pck sound table.
- wwiser resolved bank names: SBK_MC_Skate (573590595), SBK_MAD_MC_Ground_BMX (2810787281), SBK_MAD_Events_Music
  (2502911610), SBK_MAD_MC_Ground_Mountainboard (4019782771); RTPC `rtpc_bike_speed`; switch `sw_bike_speed`; `music_radio`.
- 182 radio songs = streamed stereo wems under `music_radio` in SBK_MAD_Events_Music.
- Ride sounds chosen by acoustic profile (duration, crest, centroid, flux): see sheets/rr_sounds.json.
  The same media id appears in many banks, so the runtime searches every bank for it.

## Gotchas
1. Windows MAX_PATH: deep temp paths (~243 chars) can't be opened by Python. Keep work under short paths.
2. vgmstream can't open `.pck` directly; extract the wem by AKPK index / bank DIDX first.
3. The app's per-session scratch folder was removed mid-session; the project now lives in `Documents\republic-roll`.

## Next
Preflight script, then codegen, then the Godot project, then the first vertical slice (Dust2 + FBI + skateboard + roll sound).
