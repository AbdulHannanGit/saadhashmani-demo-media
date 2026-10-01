# Saad Hashmani theme — demo media

Media used by the [Saad Hashmani WordPress theme](https://github.com/AbdulHannanGit/saadhashmani-theme) 3.x, plus the `manifest.json` its **Demo Media Importer** reads (Saad Hashmani > Theme Options).

## Layout

| Folder | Contents |
|---|---|
| `images/logo.webp`, `images/hero-still.webp` | Logo and first frame of the background video |
| `images/journey/` | The Record timeline photos |
| `images/ventures/<venture>/` | Venture logos and gallery images |
| `images/partners/` | Partner logos (marquee) |
| `images/playbook/` | Playbook reel covers (9:16) |
| `images/podcast/` | Episode posters (`<id>.webp`) and ring cards (`<id>@34.webp`) |
| `images/collage/` | Testimonial portraits |
| `video/loopscroll-{480,720,1080}p.mp4` | Background video: every section loop and transition in one file |

## manifest.json

```json
{
  "base_url": "https://raw.githubusercontent.com/AbdulHannanGit/saadhashmani-demo-media/main/",
  "files": [{ "key": "images-logo", "path": "images/logo.webp", "title": "…", "alt": "…", "sha1": "…", "bytes": 102713 }],
  "settings_map": { "options.logo": "images-logo" }
}
```

- `files` – every file once. `sha1` lets the importer skip files already in the Media Library (compared by contents, so files that only share a name are never confused) and verify downloads.
- `settings_map` – theme setting (dot path) → file `key`. One file can fill several settings.

## Rules

- **No Git LFS.** `raw.githubusercontent.com` serves LFS files as small pointer text, which the importer rejects. Keep each file under GitHub's 100 MB limit (the 1080p video is ~11 MB).
- After adding or replacing a file, update its entry (path, `sha1`, `bytes`) and any `settings_map` lines in `manifest.json`.

## Encoding the background video

The theme seeks to the start of each clip (section loops and transitions). Seeking is instant only when a keyframe sits on that frame, so the videos are encoded with a keyframe forced at every clip boundary (snapped down to the 30 fps frame grid) plus one every 2 s, and without the silent audio track:

```bash
# clip boundaries for the default clip lengths (theme: Sections > Hero > Clip lengths)
KF=0.0000,4.0323,9.0657,13.0990,18.1323,22.1990,27.2323,32.2323,37.2657,42.2657,47.2990,51.3323,56.3990,60.4323
ffmpeg -i master-1080p.mp4 -an -vf scale=854:480:flags=lanczos \
  -c:v libx264 -preset slow -profile:v high -pix_fmt yuv420p -crf 27 -maxrate 380k -bufsize 760k \
  -g 60 -keyint_min 30 -sc_threshold 0 -force_key_frames "$KF" -movflags +faststart loopscroll-480p.mp4
# 720p:  scale=1280:720  -crf 27 -maxrate 660k  -bufsize 1320k
# 1080p: scale=1920:1080 -crf 25 -maxrate 1450k -bufsize 2900k
```

If the clip lengths change, recompute `KF` (each boundary = running sum of the lengths, floored to a whole frame, minus 1 ms) and update the setting.
