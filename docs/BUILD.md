# Build and run

## Desktop

1. Install Godot **4.5.2 Standard** (GDScript) and clone this repository.
2. Import `Source/project.godot`. Allow Godot to import the textures and GLBs.
3. Press F6 on `main.tscn`, or F5 to run the project.

The `.godot` cache is generated locally. The prepared world scenes are already included.

## Android

The published APK is a signed **ARM64 review build**, version 0.41.0 / code 40, min Android API 24, target API 36. It requests VIBRATE only. Physical-phone acceptance remains pending.

For your own build, install Godot 4.5.2, Java 17, Android SDK platform/build tools 36 and platform-tools. The project includes the custom offline engine template used for this candidate. The review signing key is deliberately excluded. Generate your own local key or use your own release key; builds signed with a different key cannot update the existing review APK in place.

For a disposable local debug identity (not a production key):

```bash
keytool -genkeypair -keystore Source/build/debug.keystore -storepass android -keypass android -alias androiddebugkey -keyalg RSA -keysize 2048 -validity 10000 -dname "CN=Local Android Debug"
```

The checked-in preset uses that local path and standard debug placeholders. Keep the generated key private and out of git. Set your own release credentials locally for production.

The packaged `QA/tools/export_candidate.py` helper expects the source at `Source/` and writes `Monsoon-Dash-World-3-4.apk` at repository root. Set `MONSOON_BUILD_TOOLS` to a directory containing `Godot_v4.5.2-stable_linux.x86_64`, `ANDROID_SDK_ROOT` to the Android SDK and `JAVA_HOME` to Java 17:

```bash
python QA/tools/export_candidate.py
```

The helper exports, repairs the offline template's duplicate AndroidX startup-provider authority, aligns with 16 KB ZIP alignment and signs with your local debug identity. Do not distribute the intermediate raw export.

`QA/tools/apk_audit.py` validates the historical published certificate by default. A newly generated key will intentionally fail that certificate-continuity assertion; use the published audit for the downloadable candidate and adapt the expected certificate when maintaining a new signing identity. Static 16 KB checks do not prove physical runtime compatibility.

## Art and prepared worlds

Editable character Blender files are in `Source/art_source/`; procedural art builders are in `Source/tools/art/`. Blender 4.5.1 was used for the supplied character assets. Existing GLBs and textures allow playing without Blender.

To rebake only Worlds 3 and 4:

```bash
godot --headless --path Source --script tools/art/bake_world34.gd
```

The two approved worlds are untouched by this targeted bake. A complete bake is available in `Source/tools/art/bake_worlds.gd`.

## Tests

Examples (replace `godot` with your Godot 4.5.2 executable):

```bash
godot --headless --editor --path Source --import
godot --headless --path Source --script tests/test_rules.gd
godot --headless --path Source --script tests/test_save.gd
godot --headless --path Source --script tests/test_visual4.gd
godot --headless --path Source --script tests/test_final_ui.gd
```

Use an isolated Godot user-data directory for tests because some save tests temporarily manipulate the test save files. Do not run them against a player's live save. The Linux native-capture wrapper in QA/tools is historical and expects the original workstation's Xvfb paths; run the game normally on your own desktop for visual review.
