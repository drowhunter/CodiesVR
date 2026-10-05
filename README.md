# GridLegendsVR

A VR mod for **GRID Legends**. It is a proxy `d3d12.dll` that sits in the game
folder, hooks the game's camera and frame presentation, and shows the game in
an OpenXR headset.

- **Menus** are shown on a virtual screen in front of you.
- **On track** the view is immersive stereo with head tracking, at the
  headset's field of view. The game renders one eye per frame, alternating, so
  each eye updates at half the game's frame rate.
- The **race HUD** is lifted out of the picture and shown on a panel you can
  place, or hidden. The pause menu and the mod's own menu keep their own place.

## Features

- **Full VR for GRID Legends**: immersive stereo 3D with head tracking on
  track, in any OpenXR headset that supports Direct3D 12.
- **Menus on a virtual screen**: the game's menus and pause screen float in
  front of you at a distance and size you choose.
- **Movable race HUD**: the HUD is lifted out of the picture onto a cockpit
  panel you can position and resize, or it can be left in the corners or
  hidden.
- **In-headset settings menu**: everything is adjustable live from a menu
  usable with mouse, keyboard or controller.
- **Rebindable hotkeys**: recentre, set driving position, reset position and
  open menu can all be remapped; both thumbsticks also open the menu.
- **Guided driving-position setup**: a two-step "Set centre" to place yourself
  in the seat, plus one-key recentre.
- **Supersampling control**: pick a square render resolution suited to the
  headset, with pixels per degree shown.
- **Mixed reality background**: set a key colour around the menu screen so
  headset software can show your room through it.
- **Automatic game setup**: vsync, frame cap, motion blur and reflection
  settings are fixed for VR at each start, with your original settings backed
  up.
- **Performance readout**: an optional display of where the frame time goes.
- **One-file install**: a single `d3d12.dll`, with an installer that finds the
  Steam copy, updates and cleanly uninstalls.
- **Falls back gracefully**: with no headset connected the game runs normally
  on the monitor.

## Requirements

- GRID Legends (Steam), run at least once so that its settings file exists.
- A headset with an OpenXR runtime that supports Direct3D 12. Developed
  against Virtual Desktop.

If no headset is connected the game runs normally on the monitor and the mod
waits for one. If the headset is taken off or disconnects mid-session, the mod
stops alternating eyes until it is back.

## Install

Run `GridLegendsVR-Setup.bat` with `d3d12.dll` next to it. It finds the Steam
copy of the game, or asks you to drag the game folder onto its window and
checks that `GridLegends.exe` is there.

Run the same file again to update or uninstall. Uninstalling removes the mod's
files and puts back the game's original settings file and any `d3d12.dll` that
was in the folder before.

Run the game once without the mod first, as far as the main menu: the game
creates its settings file on first run, and the mod needs it to exist. The
installer checks and tells you if it is missing.

### Manual installation

The installer only copies one file, so you can do it by hand:

1. Close GRID Legends.
2. Open the game folder, the one that contains `GridLegends.exe`. In Steam:
   right-click the game, **Manage**, **Browse local files**. It is normally
   `...\steamapps\common\GRID Legends`.
3. If there is already a `d3d12.dll` in that folder (from another mod), rename
   it so you can put it back later.
4. Copy the mod's `d3d12.dll` into that folder.
5. Connect your headset, make sure its OpenXR runtime is active, and start the
   game from Steam as usual.

To update, replace `d3d12.dll` with the new one.

To uninstall by hand:

1. Close the game and delete `d3d12.dll` from the game folder, then rename
   any `d3d12.dll` you set aside in step 3 back.
2. Delete the files the mod created there, if present: `gridvr12.ini`,
   `gridvr12_hidden_shaders.txt`, `gridvr.log` and any `gridvr_mark_*.bmp`.
3. To get your original graphics settings back, go to
   `Documents\My Games\GRID Legends\hardwaresettings`, delete
   `hardware_settings_config.xml` and rename
   `hardware_settings_config.xml.gridvr-backup` to that name.

If you installed by hand, uninstall by hand too: the installer does not know
about a copy it did not install and would treat the mod's `d3d12.dll` as
another mod's file.

## Controls

These are the default keys. **All four can be changed** on the menu's
**Hotkeys** tab: click the key shown next to an action, then press the new
key (Esc cancels, "Clear" unbinds, "Restore default keys" puts these back).
The menu's buttons and prompts always show whichever key is currently bound.

| Default key | Action |
|---|---|
| Insert | Open / close the Grid Legends VR menu |
| Home | Recentre (position and heading) |
| End | Set centre: a guided two-step way to set your driving position |
| Page Down | Reset the driving position |

Pressing both thumbsticks in also opens the menu; that can be switched off on
the Hotkeys tab.

The menu can be driven with the mouse (it draws its own pointer; the mouse
wheel over a slider adjusts it, Ctrl for fine steps), the keyboard, or the
controller; on the controller, LB and RB switch between the menu's tabs.
While the menu is open, controller, keyboard and mouse input is
kept from the game, so navigating the menu does not also drive the game.
That can be switched off on the Hotkeys tab; steering wheels are not blocked.

Settings, including the key bindings, are saved to `gridvr12.ini` in the game
folder.

## Adjusting the in-race HUD

On track, the game's HUD (position, lap times, track map, speedometer) is
taken out of the 3D picture and shown as one flat panel fixed in the cockpit,
so it is no longer stuck in the corners of your vision.

Open the menu (Insert, or both thumbsticks) and find **Race HUD** on the
**UI** tab:

- **In the corners** leaves the HUD where the game draws it, as part of the
  picture.
- **Panel** shows it on the cockpit panel. Three sliders then appear:
  - **HUD panel distance** — how far in front of you the panel is, in metres.
  - **HUD panel width** — how wide the panel is, in metres. Smaller shrinks
    the whole HUD.
  - **HUD panel height** — how far above (positive) or below (negative) your
    recentred eye level it sits.
- **Hidden** removes the race HUD altogether.

Each slider has a text box beside it for typing an exact value, and the mouse
wheel over either one nudges it. When a slider is not at its default, a small
reload button appears between the slider and the text box; it puts the default
back. Changes show immediately; they are saved when
you close the menu or press "Save settings".

Things to know:

- The panel is placed relative to where you last recentred, so press Home
  first if the panel seems to be in the wrong place.
- These settings move only the race HUD. The pause menu and the mod's own menu
  stay at the menu-screen position ("Menu screen distance" and "Menu screen
  width" on the same tab), whatever the HUD mode.
- The HUD is one image: its elements cannot be placed individually yet.
- Dark backing plates behind HUD elements are not shown (see Known limits).

## Mixed reality background

In the menus the game is shown on a virtual screen, and what surrounds that
screen is a plain colour: black by default. Tick **Mixed Reality** on the
menu's **UI** tab to use a colour of your own (untick it to go back to black
without losing the colour). There you can pick that colour, or type its red, green and blue
values (0 to 255 each, for example 0, 255, 0 for pure green). A reload button
beside it puts black back.

This is meant for headset software that can turn one colour into passthrough
(Virtual Desktop can): set the same colour in both places and the game screen
floats in your room. Anything in the game's own picture that happens to be
that colour may become see-through too, so a strong, unusual colour is a
better key than black. Whether the colour survives the video stream well
enough to key cleanly has not been tested.

On track the 3D view fills your vision, so the colour is only visible there if
the field-of-view scale is set below 1.

## Game settings the mod forces

Each time the game starts, before the game reads its settings, the mod edits
`Documents\My Games\GRID Legends\hardwaresettings\hardware_settings_config.xml`.
The first time, it saves the untouched file beside it as
`hardware_settings_config.xml.gridvr-backup`. Changing any of these in the
game's own options lasts only until the next start.

| Setting | Forced to | Why |
|---|---|---|
| V-sync | off | The headset paces the frames. A second clock makes the game speed pulse. |
| Frame-rate limit | none (`maxFPS="0"`) | Same reason. A 60 fps cap would also mean 30 fps per eye. |
| Motion blur | off | It smears between frames that belong to different eyes. |
| Vehicle reflection updates | every frame | "Odds then evens" lines up with the alternating eyes, so each eye would get different reflections. |
| Resolution and display mode | a square size, windowed | See "Render resolution" below. Only when a size is chosen in the mod's menu, which is the default. |

Everything else in that file is left as you set it.

## Settings worth choosing yourself

- **Render resolution (the mod's menu, "Render resolution (supersampling)").**
  The headset picture is a copy of what the game renders, spread over a
  roughly square field of view, so a square resolution suits it and a 16:9 one
  wastes pixels sideways and starves the picture vertically. The menu lists
  square sizes with the resulting pixels per degree; the default is 2880 x
  2880. A change applies the next time the game starts. Do not set the
  resolution in the game's own video options: the mod overwrites it. Choose
  "Leave to the game" in the mod's menu if you want the game's setting back.
- **Frame rate matters more than usual.** Because the eyes alternate, aim for
  a game frame rate at or near the headset's refresh rate. Lower the render
  resolution or the game's detail settings before accepting a low frame rate.
  "Show performance" in the menu shows where the time goes.
- **Anti-aliasing (game options).** Not forced. With everything off the
  picture is noticeably jagged. Temporal methods (TAA, XeSS, or DLSS through
  a wrapper) reuse the previous frame, which here belongs to the other eye, so
  they may ghost or shimmer; try them and keep what looks best. SMAA does not
  depend on previous frames.
- **Frame generation.** Leave it off. It would invent frames between a
  left-eye and a right-eye image.
- **The headset runtime's own motion smoothing / reprojection** (ASW, SSW,
  SteamVR Motion Smoothing). Untested with this mod. If motion looks wrong,
  try with it off.
- **Mirrors (game options).** The game's default updates mirrors on alternate
  frames, which again coincides with the alternating eyes. Not forced, because
  the other valid values are not known; if mirrors flicker between the eyes,
  try a different mirror update setting.
- **"Show the game in the desktop window" (the mod's menu).** On by default.
  Turning it off can help performance.

## Leave these alone

- `d3d12.dll` in the game folder is the mod. Another mod that also installs
  as `d3d12.dll` cannot be used at the same time under that name.
- `gridvr12.installed` tells the installer the mod is present.
- `hardware_settings_config.xml.gridvr-backup` is what uninstalling restores.
- The window on the desktop is deliberately small when a square resolution is
  set; the game still renders at the full size. Do not resize it.

## Known limits

- Each eye updates at half the frame rate (alternate-eye rendering); fast
  motion can show a double image.
- The HUD has no transparency information of its own; the mod derives it from
  brightness, so dark backing plates disappear.
- Cutscenes and replays count as "on track" and are shown immersive.
- The start-up "go online" prompt is not skipped.
- Developed and tested on one machine, with one headset runtime.

## Troubleshooting

- **Nothing in the headset.** Check that the headset's OpenXR runtime is the
  active one, then look at `gridvr.log` in the game folder for lines starting
  `head tracker:`.
- **The view is off to one side or in the wrong seat position.** PgDn resets
  the driving position; then use End to set it again.
- **The menu is too big or too small.** "Menu size" on the menu's UI tab.
- **The view jumps to a new centre every time the game starts.** Untick
  "Recentre automatically when the game starts" on the Settings tab.
- **Something went wrong after a change.** Run the installer and choose
  Uninstall; that restores the game's files and settings.

## Building

```powershell
cmake -B build -A x64
cmake --build build --config Release --target GridVR12
```

Needs an MSVC toolset with C++20, a Windows SDK and CMake 3.21+. The first
configure fetches the Khronos OpenXR SDK and Dear ImGui, so it needs network
access.

The output is `build\Release\d3d12.dll`.

To make a folder you can hand to someone, build the `dist` target:

```powershell
cmake --build build --config Release --target dist
```

It builds the DLL if needed and writes `build\dist\GridLegendsVR\` (the DLL,
the installer, this README and a `VERSION.txt`) plus a zip of it. The version
number is `GRIDVR_VERSION` in `CMakeLists.txt`.

## Layout

| Path | What |
|---|---|
| `src/dx12-proxy/` | The `d3d12.dll` proxy and its entry point |
| `src/hooks/` | Swapchain, camera, HUD and shader hooks |
| `src/memory/` | Signature scanning and the camera callbacks |
| `src/openxr/` | The OpenXR session, stereo submission and HUD pass |
| `src/ui/overlay12.*` | The in-game menu |
| `src/vr12_settings.*`, `src/vr12_game_config.*` | The mod's settings and the game-settings edits |
| `installer/` | The install / uninstall script |
| `tools/camfind/` | Development tooling used to find the camera |

## Support

[Buy me a coffee](https://ko-fi.com/drowhunter)
