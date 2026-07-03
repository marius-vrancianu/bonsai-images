# bonsai-images

Photo storage for the [Nasty Cat Bonsai site](https://github.com/marius-vrancianu/nasty-cat-bonsai).
Images here are served to the site via CDN — this repo never needs a build.

## Layout

| Path | Purpose |
| --- | --- |
| `gallery/` | Photos shown in the site's Gallery page |
| `blog/` | Photos embedded in blog posts / the about page — **never** shown in the Gallery |
| `gallery.json` | The manifest: the Gallery shows exactly the images listed here, nothing else |

## Adding a gallery photo

1. Put the image in `gallery/` (WebP preferred, keep files under ~1 MB; jsDelivr caps files at 20 MB).
2. Add an entry to `gallery.json`:

```json
{
  "file": "gallery/tree-10.webp",
  "species": "Chinese Elm",
  "style": "Broom",
  "date": "Jul 2026",
  "ratio": "4/3",
  "alt": "Chinese elm bonsai in a shallow oval pot",
  "notes": "Longer caption shown in the lightbox."
}
```

`ratio` is the photo's aspect ratio (width/height) and controls the tile shape.
`alt` and `notes` are optional. Order in the file = order in the gallery.

## Adding a blog image

Just commit it under `blog/` — no manifest entry. Reference it from a post with:

```
{% cdnimg "blog/my-photo.webp", "Alt text", "Optional caption" %}
```

## How it's served

- Manifest: `https://raw.githubusercontent.com/marius-vrancianu/bonsai-images/main/gallery.json` (updates ~5 min after push)
- Images: `https://cdn.jsdelivr.net/gh/marius-vrancianu/bonsai-images@main/...` (jsDelivr caches up to 7 days; new files appear fast, but *replacing* an existing file under the same name can take days to propagate — prefer new filenames)
