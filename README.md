# ThingStudioWeb

The landing page for [Thingstudio](https://github.com/mkarliner/ThingStudio), served by GitHub Pages at
https://www.thingstudio.net/. One static page, no build step: what's in this repo is what's served.

- `index.html`: the page. Styles inline; fonts from Google Fonts.
- `editor.png`: the editor screenshot, copied from the product repo's `docs/user-guide/images/editor.png`.
- `CNAME`: the custom domain, `www.thingstudio.net`.
- `.nojekyll`: tells Pages to serve the files as they are.

The user guide is a separate Pages site, docs.thingstudio.net, published from the product repo.

## Where the words come from

The copy is drafted in the ThingStudioMarketing repo (`drafts/landing-page.md`) and follows its rules: every
claim is checked against the product's code and docs first, and no board is called supported until it has been
tested. Change the words there first, then here.

## Setting up Pages (once)

1. Push this repo to GitHub as a public repo.
2. Settings → Pages: source "Deploy from a branch", branch `main`, folder `/ (root)`.
3. DNS for thingstudio.net: `www` as a CNAME record to `mkarliner.github.io`. For the bare domain as well, add
   GitHub's Pages A records for `thingstudio.net`; GitHub then redirects it to `www`.
4. Back in Settings → Pages, once the domain check passes, tick "Enforce HTTPS".

## Going live

The page carries a red "Draft" strip and placeholders. Before pushing it live, remove the strip (the
`<div class="draft">` line and its CSS), fill in the contact route, and add the showreel photo or remove its slot.
