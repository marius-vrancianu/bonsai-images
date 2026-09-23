# bonsai-images

Photo storage for the [Nasty Cat Bonsai site](https://github.com/marius-vrancianu/nasty-cat-bonsai).
This repo holds the **original** photos and the gallery's manifest. It has no
build of its own: the site's deploy reads it and makes every copy visitors
see. The full owner's guide is the site repo's
[GUIDE.md](https://github.com/marius-vrancianu/nasty-cat-bonsai/blob/main/GUIDE.md)
(§1 gallery, §3 blog) — this README is the short version.

## Layout

| Path | Purpose |
| --- | --- |
| `gallery/` | Photos shown on the site's Gallery page |
| `blog/` | Photos for blog posts, post thumbnails and the About page — **never** shown in the Gallery |
| `gallery.json` | The manifest: the Gallery shows exactly the photos listed here, in this order |

## Preparing a photo

- **JPEG, WebP or PNG — not HEIC.** iPhones shoot `.heic` by default; set
  Settings → Camera → Formats → *Most Compatible*, or convert before uploading.
  The site's build cannot read HEIC.
- **1600–2000 px on the long side.** Every copy the site shows is cut from
  this file (the largest is 1800 px wide); bigger only slows deploys.
- **Optional: strip the location.** Phone photos carry the GPS position they
  were taken at, and this repo is public, so the originals here can reveal it
  (the site's own copies never do — their metadata is always removed). To
  strip it: Photoshop *Export As* with Metadata *None*, or on Windows
  right-click → Properties → Details → *Remove Properties and Personal
  Information*.
- **Name:** lowercase, no spaces. The existing convention is
  `YYYY-MM-species-<age>yo-<years in training>yt.jpg`.

## Adding a gallery photo

1. Upload the file to `gallery/`.
2. Add an entry to `gallery.json` — newest first by habit; the order here is
   the order on the page:

```json
{
	"file": "gallery/2026-07-ulmus-parvifolia-8yo-5yt.jpg",
	"species": "Ulmus parvifolia (Chinese elm)",
	"trees": ["Ulmus parvifolia, anno culto 2021"],
	"style": "Broom",
	"date": "Jul 2026",
	"notes": "Longer caption shown in the lightbox.",
	"alt": "Chinese elm with a dense broom canopy in a shallow oval pot"
}
```

| Field | Needed | Shown as |
| --- | --- | --- |
| `file` | yes | — the path in this repo |
| `species` | yes | the card's title |
| `style`, `date` | yes | the card's subtitle, "Broom · Jul 2026" |
| `trees` | no | the Gallery's "one tree over the years" filter. Always a list; copy the exact string from another photo of the same tree. A leading `+` marks a tree no longer in the collection |
| `notes` | no | the longer text in the lightbox |
| `alt` | no, but please | one plain sentence describing the photo, for screen readers |
| `ratio` | no | only to crop a card on purpose (e.g. `"1/1"`); without it the card takes the photo's own shape |

3. **Deploy the site** (site repo → Actions → *Deploy to GitHub Pages* →
   *Run workflow*). Nothing in this repo goes live until then. No waiting is
   needed after committing.

A stray or missing comma in `gallery.json`, or an entry whose file is not in
`gallery/`, **fails the deploy** — the live site stays as it was, and the
error names the problem (with line and column for a comma). Paste the file
into <https://jsonlint.com> to find a comma.

## Adding a blog or About photo

Upload it to `blog/` — no manifest entry. Reference it from a post or the
About page with:

```
{% cdnimg "blog/my-photo.jpg", "Alt text", "Optional caption" %}
```

and a post's thumbnail with `thumb: blog/my-thumb.jpg` in its front matter.
A blog photo that is not uploaded yet does **not** fail the deploy; the page
shows a hatched box with the filename until it is.

## Replacing or removing a photo

- **Replace:** upload the new version under the same name and deploy. The
  build recognises a photo by its contents, so the site picks it up.
- **Remove from the gallery:** delete its entry from `gallery.json` (the file
  can stay) and deploy. The site drops its copies on the next deploy.

## How the site uses this repo

The site's deploy workflow checks this repo out (latest commit of `main`),
reads `gallery.json`, and cuts small WebP copies of every photo it needs —
gallery thumbnails, lightbox views, blog figures and 1200 px share images —
which the site serves itself. Visitors never download anything from this
repo. A local preview of the site reads the same files over
raw.githubusercontent.com instead.
