<div align="center">

# Monsoon Dash

### Find your rhythm above the rain.

A 3D endless runner for Android, set across monsoon rooftops, lantern bazaars, waterways and cloud gardens.

**Godot 4.5.2 · GDScript · Android ARM64 · 0.41.0 Playtest**

[Download Android APK](https://github.com/Monasri29-hub/monsoon_dash/raw/refs/heads/main/downloads/Monsoon-Dash.apk) · [Watch the gameplay demo](https://github.com/Monasri29-hub/monsoon_dash/raw/refs/heads/main/docs/demo/Monsoon-Dash-Demo.mp4) · [Report a bug](https://github.com/Monasri29-hub/monsoon_dash/issues)

<img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/overview.png" alt="Monsoon Dash: home screen and gameplay across Terrace Quarter, Lantern Bazaar, Canal Works and Cloud Gardens" width="1200" />

</div>

> **Build status:** This is a playable Android review build. Automated regression and packaging checks passed; physical-phone performance, installation/save retention and final visual acceptance remain pending. The project is not a Play Store release.

## Contents

- [About the game](#about-the-game)
- [Gameplay demo](#gameplay-demo)
- [Download and installation](#download-and-installation)
- [Core gameplay](#core-gameplay)
- [Worlds and progression](#worlds-and-progression)
- [Characters and Boondi](#characters-and-boondi)
- [Workshop](#workshop)
- [Interface and settings](#interface-and-settings)
- [Controls](#controls)
- [Technology and architecture](#technology-and-architecture)
- [Run from source](#run-from-source)
- [Android builds](#android-builds)
- [Verification](#verification)
- [Known limitations](#known-limitations)
- [Contributing and feedback](#contributing-and-feedback)
- [Credits and license](#credits-and-license)
- [Media paths for maintainers](#media-paths-for-maintainers)

## About the game

Monsoon Dash follows a courier through a rain-soaked world of rooftops, bridges, gardens and waterways. The player changes lanes, avoids obstacles, collects rain tokens and opens an umbrella to glide across marked crossings.

The game combines a familiar three-lane runner loop with a monsoon setting, an expressive cloud companion named **Boondi**, and four districts with different scenery and route patterns. Canal Works introduces waterboard presentation and board/glide transitions, while the remaining districts center on running and umbrella crossings.

Project by **Kundeti Monasri**, developed through an AI-assisted design, implementation, asset-iteration and testing workflow. The repository includes the playable Godot project, editable character art, procedural art builders, regression suites and packaging evidence.

## Gameplay demo

<p align="center">
  <a href="https://github.com/Monasri29-hub/monsoon_dash/raw/refs/heads/main/docs/demo/Monsoon-Dash-Demo.mp4">
    <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/home.png" alt="Click to watch the Monsoon Dash gameplay demo" width="300" />
  </a>
  <br />
  <strong>Click the preview to watch or download the 25-second demo.</strong>
</p>

**Video source:** [docs/demo/Monsoon-Dash-Demo.mp4](https://github.com/Monasri29-hub/monsoon_dash/raw/refs/heads/main/docs/demo/Monsoon-Dash-Demo.mp4)

| Time | Scene |
|---|---|
| 00:00–00:02 | Home dashboard |
| 00:02–00:07 | Terrace Quarter |
| 00:07–00:12 | Lantern Bazaar |
| 00:12–00:17 | Canal Works |
| 00:17–00:22 | Cloud Gardens |
| 00:22–00:25 | Home and closing line |

The footage comes from the actual Godot desktop renderer with scripted lane changes and umbrella gliding. Temporary in-memory showcase routes and unlocked content were used for recording; normal progression in the APK is unchanged. The game's original synthesized menu music accompanies the footage.

The capture was sampled at 12 frames per second and encoded at 24 fps with repeated frames. It is a visual demonstration, not a phone recording or mobile-performance measurement. See [capture notes](docs/demo/CAPTURE-NOTES.txt).

## Download and installation

### Android APK

**[Download Monsoon-Dash.apk](https://github.com/Monasri29-hub/monsoon_dash/raw/refs/heads/main/downloads/Monsoon-Dash.apk)**

| Property | Current build |
|---|---|
| Version | 0.41.0 |
| Android version code | 40 |
| Application ID | `org.monsoondash.prototype` |
| Architecture | ARM64 / `arm64-v8a` |
| Minimum Android version | Android 7.0 / API 24 |
| Target API | 36 |
| APK size | Approximately 60 MiB |
| Requested permission | Vibration |
| Internet permission | Not requested |
| Distribution | Signed Android playtest APK |

1. Download the APK on a compatible Android phone.
2. Open the file in your browser's downloads or file manager.
3. If Android requests permission to install from that source, review the prompt. Keep Play Protect enabled and review any additional warning.
4. Install and open **Monsoon Dash**.
5. Follow the tutorial and start in Terrace Quarter. Later worlds and characters unlock through progression.

The downloadable APK uses the project's existing review signing identity. An independently built APK signed with a different key cannot update it in place. Do not uninstall an existing copy merely to work around a signature error, because uninstalling can remove saved progress.

### Verify the download

[Download SHA256SUMS.txt](https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/downloads/SHA256SUMS.txt)

Expected APK SHA-256:

```text
a68bc83789e71c3371f98075dd613b19c25677efd5142e85a49878524f94691e
```

From the directory containing the APK:

```powershell
# Windows PowerShell
Get-FileHash .\Monsoon-Dash.apk -Algorithm SHA256
```

```bash
# Linux
sha256sum Monsoon-Dash.apk

# macOS
shasum -a 256 Monsoon-Dash.apk
```

Compare the result with the expected value above. Source archives are developer files; install the `.apk` to play on Android.

## Core gameplay

### Running and obstacles

The runner advances along a three-lane route. Change lanes to avoid tall obstacles, jump low barriers and slide beneath beams. Route generation and collision rules are separated from the rendering code so presentation changes can be tested without changing the simulation.

### Umbrella crossings

Marked approach pads signal upcoming glide crossings. Use the jump/up command while the umbrella is ready to arm the glide, then steer left or right during the crossing. Takeoff and landing follow the authored crossing timing.

### Canal waterboard

Canal Works uses a waterboard with world-specific route presentation. The board changes between riding and glide-related states as the character opens the umbrella and crosses gaps.

### Tokens, missions and saves

Collect rain tokens, improve personal bests and complete missions. Mission templates cover activities such as token collection, glide crossings, slides, world distance and completed runs. Completed-run progression and rewards are tracked locally, with run receipts used to prevent duplicate reward commits.

Progress includes selected equipment, owned characters/cosmetics, world unlocks, best scores and settings. Save data is stored through Godot's `user://` location; the store includes validation, backup recovery and compatibility handling for earlier save versions.

## Worlds and progression

World unlock thresholds use accumulated distance in saved progression, rather than requiring each threshold in a single run.

| World | Unlock threshold | Identity |
|---|---:|---|
| Terrace Quarter | Available initially | Rain-soaked rooftop terraces and warm stone architecture |
| Lantern Bazaar | 3,000 m | Lantern-lit buildings and slate routes |
| Canal Works | 8,000 m | Waterboard routes, lock houses, sluices and copper machinery |
| Cloud Gardens | 15,000 m | Planted terraces, pavilions and pale stone garden paths |

### World 1 — Terrace Quarter

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/terrace.png" alt="Terrace Quarter gameplay with rooftop architecture, rain, Tara and Boondi" width="320" />
</p>

The starting district introduces the core running and umbrella-crossing loop. Rooftop terraces, parapets, plants and warm architectural colors establish the monsoon-city setting.

### World 2 — Lantern Bazaar

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/bazaar.png" alt="Lantern Bazaar gameplay with slate paths and lantern-lit architecture" width="320" />
</p>

Lantern Bazaar pairs its own track patterns with lantern-lit architecture, warm accents and darker slate paths. It extends the established running mechanics into a different district atmosphere.

### World 3 — Canal Works

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/canal.png" alt="Canal Works gameplay showing Tara on a waterboard beside sluice and pump structures" width="320" />
</p>

Canal Works combines waterboard routes with stepped stone lock houses, arched sluice bays, pumps, copper valves and service railings. Its 0.41.0 visual update adds revised water shading, layered skyline scenery, cool mist and a teal district badge.

### World 4 — Cloud Gardens

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/gardens.png" alt="Cloud Gardens gameplay with planted terraces, pavilions and pale stone paths" width="320" />
</p>

Cloud Gardens uses planted terraces, pavilions, pergolas, hanging vines, fountains and wind-leaf details. Warmer limestone paths and a lavender skyline distinguish the district from the city and canal areas.

The 0.41.0 update targeted Canal Works and Cloud Gardens. The 28 prepared World 1/2 scenes were retained byte for byte from the preceding candidate.

## Characters and Boondi

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/characters.png" alt="Monsoon Dash character selection screen" width="360" />
</p>

| Character | Role | Unlock cost |
|---|---|---:|
| Tara | Terrace courier | Available initially |
| Veer | Waterboard inventor | 800 tokens |
| Isha | Garden cartographer | 1,400 tokens |
| Kabir | Community courier | 2,000 tokens |

Character selection, ownership and outfit choices are maintained through the catalogue and save systems. The character assets include rigged GLBs, animations, textures and editable Blender sources.

**Boondi** accompanies the runner as an expressive cloud character. Its presentation includes relaxed, delighted and surprised reactions, plus movement and token-related visual responses. The companion can be shown or hidden in Settings. These reactions are presentation features; they are not unimplemented shields, revives or other gameplay powers.

## Workshop

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/workshop.png" alt="Workshop screen for character outfits, umbrellas and waterboards" width="360" />
</p>

The workshop supports purchasing and equipping cosmetic items using **in-game tokens**:

| Category | Examples | Cost per item |
|---|---|---:|
| Character outfits | Coral Showers, Workshop Blue, Night Chart, Skyline Navy | 300 tokens |
| Umbrellas | Paper Kite, Cloudline, Copper Rain | 400 tokens |
| Waterboards | River Mint, Sunset Coral | 450 tokens |

The interface distinguishes locked, owned and selected items. Character-specific outfits require ownership of the corresponding character. No real-money payment system is claimed.

## Interface and settings

The home dashboard provides access to character selection, the world map, workshop, missions, practice and settings. During a run, the HUD shows score, distance, tokens, district information and contextual crossing cues. Pause and results screens support the run flow.

<p align="center">
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/home.png" alt="Home dashboard with Tara, Boondi, navigation and world cards" width="300" />
  <img src="https://raw.githubusercontent.com/Monasri29-hub/monsoon_dash/main/docs/screenshots/settings.png" alt="Settings screen with audio, rain, motion and control preferences" width="300" />
</p>

Available preferences include music/sound levels, vibration, optional button controls, companion visibility, reduced motion, and independent **Full / Light / Off** rain quality. Practice/tutorial access helps players learn the controls. Safe-area and compact-screen layouts have automated coverage; device testing remains necessary.

## Controls

| Action | Android | Desktop |
|---|---|---|
| Change lane / steer during glide | Swipe left or right | Left/right arrows or A/D |
| Jump / arm an available umbrella crossing | Swipe up | Up arrow, W or Space |
| Slide | Swipe down | Down arrow or S |
| Pause / return | Pause button / Android Back | Escape / pause button |

Enable **Button controls** in Settings to use visible controls instead of relying only on swipes.

## Technology and architecture

| Area | Implementation |
|---|---|
| Engine and runtime | Godot 4.5.2 Standard, GDScript |
| Characters | Rigged GLBs, packed textures, editable Blender art |
| Art authoring | Blender 4.5.1 and procedural GDScript builders |
| Environments | Prepared world meshes, scenery LODs and district shaders |
| Persistence | Validated local JSON saves through `user://` |
| Audio | Original deterministic compositions and synthesized Foley |
| Android build tooling | Godot export, Java 17, Android SDK/build tools 36 |
| Verification | GDScript regression suites and Python packaging checks |

### Main modules

| File | Responsibility |
|---|---|
| [`main.gd`](Source/scripts/main.gd) | Application orchestration, input and screen/world coordination |
| [`run_model.gd`](Source/scripts/run_model.gd) | Gameplay simulation, movement, obstacle interaction and crossings |
| [`progression.gd`](Source/scripts/progression.gd) | Missions, accumulated progress and world unlock rules |
| [`save_store.gd`](Source/scripts/save_store.gd) | Persistence, validation, recovery and committed rewards |
| [`catalogue.gd`](Source/scripts/catalogue.gd) | Character/cosmetic definitions and ownership validation |
| [`interface.gd`](Source/scripts/interface.gd) | Menus, HUD, results and settings presentation |
| [`home_dashboard.gd`](Source/scripts/home_dashboard.gd) | Home dashboard composition |
| [`world_art.gd`](Source/scripts/world_art.gd) / [`world_cache.gd`](Source/scripts/world_cache.gd) | World art routing and prepared resources |
| [`world34_detail.gd`](Source/scripts/world34_detail.gd) | Canal/Gardens decorative scenery builders |
| [`tara_rig.gd`](Source/scripts/tara_rig.gd) / [`board_rig.gd`](Source/scripts/board_rig.gd) | Character and waterboard presentation |
| [`support_art.gd`](Source/scripts/support_art.gd) | Boondi rig and reactions |
| [`rain_field.gd`](Source/scripts/rain_field.gd) / [`audio_director.gd`](Source/scripts/audio_director.gd) | Weather and audio presentation |

Decorative world art is kept separate from collision rules. Water animation follows the simulation clock and reduced-motion preference, while prepared assets reduce the need to build scenery repeatedly at runtime.

### Repository layout

| Location | Contents |
|---|---|
| `Source/project.godot` | Project entry point |
| `Source/main.tscn` | Main scene |
| `Source/scripts/` | Runtime and presentation code |
| `Source/assets/` | Characters, UI, textures, shaders and prepared worlds |
| `Source/art_source/` | Editable Blender character art and references |
| `Source/tools/art/` | Art generators and world-baking tools |
| `Source/tests/` | Gameplay, progression, save, art and UI suites |
| `Source/build/` | Custom engine export templates; private signing keys must be excluded |
| `Source/audio/` | Music, ambience, effects and rights notes |
| `QA/` | Verification evidence, native captures and packaging helpers |
| `docs/screenshots/` | README screenshots and overview image |
| `docs/demo/` | Demo video and capture notes |
| `downloads/` | Android APK and checksum |

## Run from source

### Requirements

- Godot **4.5.2 Standard** for the supplied GDScript project.
- A desktop capable of running Godot's renderer.
- Blender **4.5.1** only if editing or rebuilding character art.

### Open in the editor

```bash
git clone https://github.com/Monasri29-hub/monsoon_dash.git
cd monsoon_dash
godot --editor --path Source
```

Alternatively, open Godot's project manager, select **Import**, and choose `Source/project.godot`. Allow Godot to finish importing textures and GLBs, then press **F5**.

If `godot` is not on your PATH, replace it with the full path to your Godot executable. The generated `.godot` import cache is not part of the source distribution. Existing GLBs and prepared world scenes allow the game to run without rebuilding the Blender assets.

When using the separately distributed files, extract the full source archive and the showcase kit into one project folder so `Source/`, `QA/`, `docs/` and `downloads/` are siblings.

## Android builds

Install Godot 4.5.2, **Java 17**, Android SDK/platform tools and **build tools 36**. Configure the Godot Android export environment. The source includes the custom offline engine template used for this candidate.

The review signing key must remain private and excluded from the repository. Use your own local signing identity for new builds. A different key creates a separate update identity from the downloadable review APK.

The supplied packaging helper is:

```bash
python QA/tools/export_candidate.py
```

It expects the `Source/` folder layout and the configured tool locations. See [docs/BUILD.md](docs/BUILD.md) for environment variables, local signing and exact setup.

The helper also applies a narrow repair for the offline template's duplicate AndroidX startup-provider authority, then aligns and signs the APK. Do not distribute its intermediate raw export. Certificate-continuity assertions in the historical audit are specific to the published review identity and must be updated intentionally when maintaining a different signing identity.

### Rebuild Worlds 3 and 4 only

```bash
godot --headless --path Source --script tools/art/bake_world34.gd
```

This targeted bake prepares Canal Works and Cloud Gardens assets while retaining the approved World 1/2 scenes. Character and other world builders are available under `Source/tools/art/`.

## Verification

### Checks completed for 0.41.0

| Suite / inspection | Result |
|---|---|
| Geometry and gameplay-corridor checks | 262 checks passed |
| Final-art regression suite | 69 checks passed |
| Prepared-art/resource suite | 50 checks passed |
| Final-UI regression suite | 174 checks passed |
| HUD bounds | Passed at two phone sizes |
| Desktop screenshot review | 480 × 854 and 320 × 568 |
| World 1/2 prepared scenery | 28 scenes retained byte for byte |
| APK world-cache comparison | 56 scenes matched built source |
| Character/portrait import records | 57 records checked against built source |
| Android packaging | Version, architecture, signature continuity, permissions and provider authorities checked |
| Alignment | ZIP and static native 16 KB checks passed |

See [test results](QA/test-results.json), [APK audit](QA/apk/apk-audit.json), [runtime asset verification](QA/apk/runtime-assets.json) and [the current world update](Source/WORLD-3-4-UPDATE.md).

These are scoped results for the recorded candidate. They do not claim that every historical suite was rerun for this visual-only update.

### Run selected suites

```bash
godot --headless --editor --path Source --import
godot --headless --path Source --script tests/test_rules.gd
godot --headless --path Source --script tests/test_save.gd
godot --headless --path Source --script tests/test_visual4.gd
godot --headless --path Source --script tests/test_final_ui.gd
```

Use an isolated Godot user-data environment for tests. Some save tests temporarily manipulate test save files; do not run them against a player's live save.

## Known limitations

- Sustained phone performance, touch feel and device-specific presentation still require physical-device review.
- Installation/update save retention has not been accepted on a physical phone for this candidate.
- Static 16 KB alignment checks do not prove runtime compatibility on a physical 16 KB device.
- The demo is a scripted desktop capture with showcase unlocks, not a recording of normal progression on a phone.
- Menu world cards use concept illustrations; gameplay screenshots show the implemented renderer.
- No hosted browser-playable build, cloud leaderboard, online multiplayer or real-money payment system is claimed.
- An owner-signed Play Store release and Android App Bundle remain separate release work.

## Contributing and feedback

Please use [GitHub Issues](https://github.com/Monasri29-hub/monsoon_dash/issues) for bugs and suggestions. A useful report contains:

1. Game version, phone model and Android version, or desktop OS/Godot version.
2. The world or screen where the issue occurred.
3. Reproduction steps and expected versus actual behavior.
4. A screenshot or short recording, with personal information removed.

For source contributions, describe the concrete behavior being changed, keep changes focused and run the relevant suites. Changes to gameplay timing, saves or rewards need simulation/progression checks; visual changes need renderer review and corridor checks. Keep keys, credentials, generated caches and private player saves out of commits.

## Credits and license

**Project:** Kundeti Monasri ([Monasri29-hub](https://github.com/Monasri29-hub)).

The project was developed through an AI-assisted workflow for design, implementation, asset iteration and testing. Editable sources and verification evidence are included so the implementation can be inspected and extended.

- **Godot:** engine under its included MIT notice, [`Source/GODOT_LICENSE.txt`](Source/GODOT_LICENSE.txt).
- **Characters and scenery:** project-authored editable art, GLBs, textures and procedural builders.
- **Background/concept art:** project-generated scenery and user-supplied design-document panels, documented in [`RIGHTS.md`](Source/assets/ui/home/RIGHTS.md).
- **Font:** Bangers by Vernon Adams, under the included [SIL Open Font License](Source/assets/fonts/Bangers-LICENSE.txt).
- **Audio:** original deterministic compositions and synthesized Foley; see [audio rights](Source/audio/RIGHTS.md).
- **Screenshots/video:** actual desktop game renderer; the overview is a contact sheet assembled from captures.

The repository's existing [MIT license](LICENSE) applies to project code. Third-party components retain their own license terms. See [docs/CREDITS.md](docs/CREDITS.md) for provenance details.

## Media paths for maintainers

All image `src` attributes above use this repository's raw `main` URLs. The linked video preview uses the actual `.mp4` path rather than relying on an HTML video player inside GitHub Markdown.

| Asset | Required repository path |
|---|---|
| Overview / hero | `docs/screenshots/overview.png` |
| Demo preview / home | `docs/screenshots/home.png` |
| Terrace Quarter | `docs/screenshots/terrace.png` |
| Lantern Bazaar | `docs/screenshots/bazaar.png` |
| Canal Works | `docs/screenshots/canal.png` |
| Cloud Gardens | `docs/screenshots/gardens.png` |
| Character roster | `docs/screenshots/characters.png` |
| Workshop | `docs/screenshots/workshop.png` |
| Settings | `docs/screenshots/settings.png` |
| Demo video | `docs/demo/Monsoon-Dash-Demo.mp4` |
| Demo notes | `docs/demo/CAPTURE-NOTES.txt` |
| Android installer | `downloads/Monsoon-Dash.apk` |
| APK checksum | `downloads/SHA256SUMS.txt` |

**Publication checklist:** Save this file as `README.md` at the repository root. Merge the full source package's `Source/` and `QA/` folders with the showcase kit's `docs/` and `downloads/` folders. Upload the files using the exact paths above. Media and installer links become live only after those files are pushed; writing this README does not itself publish them.

For distribution through GitHub Releases, attach the APK to a release and replace the APK button with that asset's actual published download URL. Do not publish private signing keys from a development archive. Preserve the provided credits and licenses.
