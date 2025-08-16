# 3D-tracking
3D tracking using COLMAP

# PRD: Windows (CUDA) Automated 3D Camera Tracking — Video or Image Sequences → COLMAP → Blender

> Local, single-click pipeline. Input a video or a time/hyper-lapse photo sequence. Output a ready-to-import COLMAP workspace and a Blender scene with animated camera + sparse point cloud.

---

## 1) References

* COLMAP repo and Windows releases. ([GitHub][1])
* COLMAP Windows CUDA artifact. ([GitHub][2])
* COLMAP CLI docs and FAQ (GPU auto-use). ([COLMAP][3])
* FFmpeg official downloads and Windows builds. ([FFmpeg][4], [Gyan.dev][5])
* Polyfjord batch script (automation). ([Gist][6])
* Blender Photogrammetry Importer (SBCV) + install docs. ([GitHub][7], [Blender Addon Photogrammetry Importer][8])
* Blender backend preference and proxy docs. ([docs.blender.org][9])
* Tutorial transcript cues used below. &#x20;

---

## 2) Goal

* Accept `.mp4/.mov/.mkv` or image sequences (`.jpg/.png`, folder or ZIP).
* Produce a COLMAP workspace: `images/`, `sparse/0/`, `database.db`.&#x20;
* Import cleanly into Blender with Photogrammetry Importer. Backend must be **OpenGL** for point visibility.&#x20;

---

## 3) Users

* Non-programmers in VFX/design.
* Power users who batch many clips.

---

## 4) Success metrics

* 1 input → 1 scene folder with animated camera in Blender in ≤3 user actions.
* 90% of outdoor handheld or tripod clips reconstruct without manual tracks.&#x20;
* Import path requires no manual file picks beyond folder select; “Suppress distortion warnings” enabled.&#x20;

---

## 5) Scope (MVP)

* Video → frames via FFmpeg; image sequences ingested as-is.  ([FFmpeg][4])
* COLMAP CLI: `feature_extractor` → `exhaustive_matcher` → `mapper` (GPU if present). ([COLMAP][3])
* Output scene packaging per clip.
* Blender import guide + OpenGL backend switch + proxy recommendation.  ([docs.blender.org][10])

Out of scope: dense MVS, scaling to meters, mesh cleanup.

---

## 6) Non-functional

* Local only.
* Windows 10/11. NVIDIA GPU recommended; use **COLMAP x64 Windows CUDA** build. ([GitHub][2])
* COLMAP uses CUDA automatically when available; otherwise CPU flags exist. ([COLMAP][11])

---

## 7) Folder layout (required)

```
AutomatedTracker/
  01_colmap/      # COLMAP CUDA build contents
  02_videos/      # input videos (or leave empty if using images)
  03_ffmpeg/      # FFmpeg binaries
  04_scenes/      # per-clip outputs
  05_script/      # batch_reconstruct.bat
```

Exact names are required by the script.&#x20;

---

## 8) Ingest

**Inputs**

* Single video file in `02_videos/`.
* OR a folder/ZIP of photos from time/hyper-lapse placed/extracted under `04_scenes/<job>/images/` before reconstruction.

**Validation**

* Frame count ≥ 60 or image count ≥ 60.
* Min resolution 1080p or 12MP for photos.

**Capture tips**

* Disable stabilization; COLMAP needs true motion.&#x20;

---

## 9) Pipeline

1. **Frame extraction** (videos): FFmpeg writes `/images/%06d.jpg`.  ([FFmpeg][4])
2. **Features + matching + mapping**: COLMAP CLI writes `database.db` and `sparse/0/`. ([COLMAP][3])
3. **Packaging**: ensure the scene folder contains:

   * `images/`, `sparse/0/`, `database.db`, `manifest.json`.&#x20;
4. **Idempotence**: skip videos already processed. ([Gist][6])

---

## 10) Blender handoff (user steps shown in UI)

1. Install **Photogrammetry Importer** from GitHub release. Don’t unzip; install the ZIP. ([GitHub][12], [Blender Addon Photogrammetry Importer][8])&#x20;
2. `File → Import → COLMAP (model/workspace)` → pick the scene folder → enable **Suppress distortion warnings** → Import.&#x20;
3. If points are invisible, `Preferences → System → Display Graphics → Backend = OpenGL` and restart Blender.  ([docs.blender.org][9])
4. If playback stutters, build **proxies** at 25–50%. ([docs.blender.org][10])

---

## 11) UX flow

* **Home**: “Process all videos in `02_videos`” and “Process images in a folder.”
* **Run**: stage bar: Extract → Features → Match → Map → Package.
* **Open in Blender**: copy-paste import steps above.
* **Logs**: link to `04_scenes/<clip>/log.txt`.

---

## 12) Controls (advanced)

* `frame_step`: process every Nth frame for long videos.
* `max_images`: cap frames.
* SIFT quality preset.
* Toggle “points as mesh” note for Blender import.&#x20;

---

## 13) Risks

* Long clips create huge image sets. Offer `frame_step`.
* Vulkan viewport issues. Keep OpenGL for this workflow.&#x20;
* CPU fallback slower. Surface ETA; advise CUDA build. ([GitHub][2], [COLMAP][11])

---

## 14) Non-programmer setup (Windows, CUDA)

1. **Download COLMAP (CUDA build)** → unzip into `01_colmap/`. ([GitHub][2])
2. **Download FFmpeg “release essentials” (Windows)** → unzip into `03_ffmpeg/`. ([FFmpeg][4], [Gyan.dev][5])
3. **Download batch script** → save as `05_script\batch_reconstruct.bat`. ([Gist][6])
4. Put your video(s) in `02_videos\`. For photo sequences, place them in `04_scenes\<job>\images\` then run step 5 to do COLMAP only.&#x20;
5. **Double-click** `batch_reconstruct.bat`. It extracts frames, runs COLMAP, and writes `images/`, `sparse/0/`, `database.db` under `04_scenes\<clip>\`.&#x20;
6. In Blender, install **Photogrammetry Importer** and import the **COLMAP model/workspace**. Enable **Suppress distortion warnings**. If the cloud is missing, switch backend to **OpenGL**.&#x20;

---

## 15) CLI appendix (for power users)

* FFmpeg extraction example:
  `ffmpeg -y -i input.mp4 -qscale:v 2 images/%06d.jpg` ([FFmpeg][4])
* COLMAP sequence:
  `colmap feature_extractor --database_path database.db --image_path images`
  `colmap exhaustive_matcher --database_path database.db`
  `colmap mapper --database_path database.db --image_path images --output_path sparse` ([COLMAP][3])

---

## 16) Definition of Done

* For a sample Himalaya clip or sequence, `04_scenes/<name>/` contains `images/`, `sparse/0/`, `database.db`. Import succeeds with visible points and an animated camera in Blender.&#x20;

[1]: https://github.com/colmap/colmap?utm_source=chatgpt.com "COLMAP - Structure-from-Motion and Multi-View Stereo"
[2]: https://github.com/colmap/colmap/releases?qUcTJX=EUlkEVRrf0Az&utm_source=chatgpt.com "Releases · colmap/colmap"
[3]: https://colmap.github.io/cli.html?utm_source=chatgpt.com "Command-line Interface — COLMAP 3.13.0.dev0"
[4]: https://www.ffmpeg.org/download.html?utm_source=chatgpt.com "Download FFmpeg"
[5]: https://www.gyan.dev/ffmpeg/builds/?utm_source=chatgpt.com "Builds - CODEX FFMPEG @ gyan.dev"
[6]: https://gist.github.com/polyfjord/4ed7e8988bdb9674145f1c270440200d?utm_source=chatgpt.com "Batch script for automated photogrammetry tracking workflow"
[7]: https://github.com/SBCV/Blender-Addon-Photogrammetry-Importer?utm_source=chatgpt.com "SBCV/Blender-Addon-Photogrammetry-Importer"
[8]: https://blender-photogrammetry-importer.readthedocs.io/en/latest/installation.html?utm_source=chatgpt.com "Installation Instructions — Blender-Addon-Photgrammetry-Importer 2.0.0 documentation"
[9]: https://docs.blender.org/manual/en/latest/editors/preferences/system.html?utm_source=chatgpt.com "System - Blender 4.5 LTS Manual"
[10]: https://docs.blender.org/manual/en/latest/editors/video_sequencer/sequencer/sidebar/proxy.html?utm_source=chatgpt.com "Proxy - Blender 4.5 LTS Manual"
[11]: https://colmap.github.io/faq.html?utm_source=chatgpt.com "Frequently Asked Questions — COLMAP 3.13.0.dev0"
[12]: https://github.com/SBCV/Blender-Addon-Photogrammetry-Importer/releases?utm_source=chatgpt.com "Releases · SBCV/Blender-Addon-Photogrammetry-Importer"
