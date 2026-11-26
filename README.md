# ThingStudioWeb

The ThingStudio site, [thingstudio.net](https://thingstudio.net/): the landing page plus News, Logs and Demos.
Built with [Hugo](https://gohugo.io/) (extended, 0.152.2) and published to GitHub Pages by
`.github/workflows/pages.yml` on every push to `main`.

The user guide is a separate Pages site, docs.thingstudio.net, published from the product repo. Step-by-step
tutorials go there; project logs (dated build stories) go here.

## Working on it

    hugo server -D        # http://localhost:1313, drafts included, reloads on save
    hugo new logs/camera-node.md
    hugo new demos/encoder-volume.md
    hugo new announcements/v0-13.md

New posts start as drafts (`draft: true`); delete that line to publish. Each section has an RSS feed
(`/logs/index.xml` etc.) and `/index.xml` carries everything.

## Layout

- `static/index.html`: the landing page. Served as it is, not through a template, so it's still one
  hand-written page with inline styles. Its nav and footer are written out in it, so new sections need adding
  there by hand.
- `content/announcements/`, `content/logs/`, `content/demos/`: the posts, one markdown file each. Front matter:
  `title`, `date`, `description`, `boards`, `tags`. Only list boards that have been tested.
- `layouts/`: templates for the section pages (`_default/`, `partials/`) and the `video` shortcode.
- `static/css/site.css`: styles for the section pages, copied from the landing page's tokens. Change colours or
  fonts in both places.
- `static/css/syntax.css`: code highlighting, generated with `hugo gen chromastyles` (github / github-dark).
- `archetypes/`: the starting front matter for `hugo new`.
- `static/media/`: images and short clips for posts.

## Video

    {{< video src="/media/encoder-volume" poster="/media/encoder-volume-poster.jpg" loop="true" >}}

`src` without an extension offers `.webm` then `.mp4`. `loop="true"` is for short silent clips (autoplays muted
unless the visitor prefers reduced motion); without it the video has controls and loads nothing until played.
No third-party players.

Keep clips in the repo small: 720p, a few MB, under 30 s. Anything over about 5 MB goes to external storage
and `src` becomes its full URL. GitHub Pages refuses files over 100 MB and the site should stay well under 1 GB.
Git LFS doesn't work with Pages.

## Static files (in `static/`)

- `fonts/`: the three typefaces (latin subsets, SIL OFL; licences beside them), served from here rather than
  Google Fonts so a visit makes no third-party request for type (2026-11-08).
- `blink-demo.mp4`, `blink-demo.webm`: the hero video (21 s loop, no sound): the blink flow built in the demo
  editor (v0.12.1), compiled, and its MicroPython shown. Masters and the GIF version are in ThingStudioMarketing
  `assets/thingstudio-blink-demo.*`.
- `blink-demo-poster.jpg`: the video's last frame, shown before it plays and when reduced motion is set.
- `editor-flow.png`: the old hero screenshot (v0.8.0, a sensor flow), no longer used by the page.
- `editor-tour.png`: the labelled editor tour, linked from the hero video's caption; copied from the product
  repo's `docs/user-guide/images/editor-parts.png`.
- `og.png`: the 1200x630 link-preview image for Hackaday.io, the fediverse and LinkedIn.
- `favicon.svg`: the tab icon.
- `editor.png`: the old full-window screenshot, no longer used by the page.
- `CNAME`: the custom domain, `thingstudio.net` (canonical; www redirects to it). With Actions publishing the
  domain is set in Settings → Pages; the file is kept as a record.
- `.nojekyll`: left from branch publishing; harmless.

## Where the words come from

The copy is drafted in the ThingStudioMarketing repo (`drafts/landing-page.md`) and follows its rules: every
claim is checked against the product's code and docs first, and no board is called supported until it has been
tested. Change the words there first, then here.

## Setting up Pages (once)

1. Push this repo to GitHub as a public repo.
2. Settings → Pages: source "GitHub Actions" (was "Deploy from a branch" until the move to Hugo).
3. DNS for thingstudio.net: `www` as a CNAME record to `mkarliner.github.io`. For the bare domain as well, add
   GitHub's Pages A records for `thingstudio.net`; GitHub then redirects it to `www`.
4. Back in Settings → Pages, once the domain check passes, tick "Enforce HTTPS".

## Going live

Live. The GUI nodes were left out while provisional (Mike, 2026-10-26); since 2026-11-16 the page mentions them
as early-stage, in the Flows row (Mike).
