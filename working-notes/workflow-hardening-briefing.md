# Briefing: harden the Pages publish workflow

2026-11-28. Status: **brief, not started.** Branch: `workflow-hardening` (cut from `site/hugo`, where
`.github/workflows/pages.yml` lives; `main` doesn't have the Hugo workflow yet, and has three newer landing-page
commits that `site/hugo` lacks — merging those is Mike's call, not part of this job). Effort: about 30 minutes.

## Why

Supply-chain attacks are one of Mike's top concerns (see the ThingStudio repo's `docs/security-policy.md`, where the
supply chain is a Defend tier). The same hardening has just been done for the COSRT50 site. Today's `pages.yml` has
three gaps:

1. **Hugo is downloaded and installed without checking it.** `curl … hugo_extended_0.152.2_linux-amd64.deb` then
   `dpkg -i`, with no checksum.
2. **Actions are pinned by tag** (`@v4`, `@v5`, `@v3`). A tag can be moved to different code.
3. **The build job holds the publish token.** `pages: write` and `id-token: write` are set at workflow level, so
   every job gets them, including `build`, which runs Hugo over the repo.

## What to change

All in `.github/workflows/pages.yml`, keeping the same tool and Action versions as today (no upgrades in this change).

- **Verify Hugo.** Add `HUGO_SHA256` beside `HUGO_VERSION` and check the download before installing:

  ```yaml
  env:
    HUGO_VERSION: 0.152.2
    HUGO_SHA256: 13f81e8ef6c427422d06e574272d66cc3d5bba9bbc254900e51dc61d806608a0
  …
  run: |
    curl -sSLf -o hugo.deb "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb"
    echo "${HUGO_SHA256}  hugo.deb" | sha256sum -c -
    sudo dpkg -i hugo.deb
  ```

  The hash is the `hugo_extended_0.152.2_linux-amd64.deb` line of the release's `hugo_0.152.2_checksums.txt`
  (checked 2026-11-28). Re-check it against that file when implementing.

- **Pin Actions to commit SHAs**, version in a trailing comment. These are the commits today's tags point to
  (resolved 2026-11-28 with `git ls-remote`; re-resolve when implementing):

  | Action | Pin |
  |---|---|
  | `actions/checkout` | `11d5960a326750d5838078e36cf38b85af677262 # v4.4.0` |
  | `actions/upload-pages-artifact` | `56afc609e74202658d3ffba0e8f6dda462b719fa # v3.0.1` |
  | `actions/deploy-pages` | `d6db90164ac5ed86f2b6aed7e0febac5b3c0c03e # v4.0.5` |
  | `actions/configure-pages` | `983d7736d9b0ae728b81ab479565c72886d7745b # v5.0.0` — but see below |

- **Drop `configure-pages` from the build.** It's only used for `base_url`, and `hugo.toml` already has
  `baseURL = "https://thingstudio.net/"`. Build with plain `hugo --gc --minify`. One fewer Action, and the build job
  then needs no Pages permission at all.

- **Move permissions to the jobs.** Workflow level: `contents: read` only. `build`: `contents: read`. `deploy` (runs
  no repo code): `pages: write`, `id-token: write`.

- **`actions/checkout` with `persist-credentials: false`**, so the token isn't left in `.git/config` for later steps.

- **Write the rule down.** Add a short "Dependencies" paragraph to `README.md`: Hugo's version and SHA-256 are
  pinned in the workflow and change together (copy both from the new release's checksums file); Actions are pinned
  to SHAs; adding or changing either needs Mike's approval. Also note in the README's opening that the workflow
  verifies Hugo.

## Checks

- YAML parses; `grep -n 'uses:' .github/workflows/pages.yml` shows only 40-character SHAs.
- The checksum step fails on a wrong hash: test it once in a scratch copy, not in the repo.
- After Mike merges and pushes, the Actions run goes green and https://thingstudio.net/ is unchanged.

## Ground rules for the session

- Stay on this branch. Hand Mike any `git add`/`git commit` (and `rm -f .git/index.lock` first); a sandbox commit
  was blocked on 2026-11-28. Commands for Mike: zsh on macOS, no `#` comments in command blocks.
- Don't run Hugo from the agent sandbox against this live-mounted folder; build in a fresh copy in the cloud
  workspace if a build check is wanted.
