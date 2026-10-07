# republic-roll
Grab a skateboard or a BMX and cruise Counter-Strike 2's maps like never before. No bombs, no timer, just you, the ride and the streets of Dust II and Mirage.

> **Status: in development.** There is no playable build yet. This repository holds the design sheets and,
> soon, the source. Releases will appear here and on Melty once a version has been built and tested.

## What it will be
- **Real CS2 maps and agents:** Dust II and Mirage, ridden as the FBI, SAS, Phoenix or Elite Crew, loaded from your own CS2 install.
- **Two rides:** push and ollie on a skateboard, or pedal and bunny-hop on a BMX.
- **Riders Republic's radio:** its soundtrack plays from your own copy while you ride (N skips a track).
- **Riders Republic ride sounds:** its real skate and BMX wheel, landing and wind sounds follow your speed.
- **Riders Republic on the big screen:** its trailers and event videos play on billboards around the maps.

Single player first; riding together is planned for a later update.

## Requirements
- **Counter-Strike 2** (Steam).
- **Riders Republic** (Steam) for the music, ride sounds and videos. Without it you can still ride, and the game tells you what's missing.

## Safe for your accounts
It runs as its own program and only *reads* your game files. It never starts, injects into or modifies CS2 or
Riders Republic, so VAC and BattlEye are never involved. No game files are included in this repository or in
releases: everything is converted from your own installs on first launch.

## How it's built
- `sheets/` - the design, one JSON table per kind of thing (maps, agents, rides, sounds, music, videos, billboards).
  The code is generated from and checked against these.
- `MODLOG.md` - research notes: file formats, IDs and how each was verified.

## Credits
- [Source 2 Viewer / ValveResourceFormat](https://github.com/ValveResourceFormat/ValveResourceFormat) (MIT) - reads CS2 maps and agents.
- [vgmstream](https://github.com/vgmstream/vgmstream) - decodes Riders Republic's Wwise audio.
- [wwiser](https://github.com/bnnm/wwiser) - used during research to map Riders Republic's sound banks.
- [FFmpeg](https://ffmpeg.org) (LGPL build) - converts Riders Republic's videos.
- [Godot Engine](https://godotengine.org) (MIT).
- Made with AI assistance (Claude).

Counter-Strike 2 is a trademark of Valve Corporation. Riders Republic is a trademark of Ubisoft. This is an
unofficial fan project, not affiliated with or endorsed by either.
