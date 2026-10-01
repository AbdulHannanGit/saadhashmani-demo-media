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
