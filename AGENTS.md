# AGENTS.md — headline-markstos fork maintenance

This is a personal fork of [TryGhost/Headline](https://github.com/TryGhost/Headline),
a Ghost CMS theme. The fork lives on the `markstos` branch; `upstream/main` tracks
the original repo.

## Remotes

```
origin    git@github.com:markstos/Headline-markstos.git   (fork)
upstream  git@github.com:TryGhost/Headline.git             (upstream)
```

## Setup

```
yarn install
```

## Build (required before every commit)

`yarn dev` starts a watcher that never exits. For a one-shot build, use:

```
npx gulp build
```

This regenerates `assets/built/screen.css`, `assets/built/main.min.js`, and merges
locale files. Always run it before committing if any source files changed.

## Test

```
yarn test   # runs gscan
```

## Rebasing onto upstream

Check what upstream has that the fork does not:

```
git fetch upstream
git log --oneline upstream/main ^markstos
```

If there are new commits, rebase:

```
git rebase upstream/main
```

Conflicts are rare but most likely in `package.json` (deps), `yarn.lock`, and `.hbs`
template files. After resolving any conflicts:

1. Run `yarn install` to regenerate `yarn.lock` for any new/changed deps.
2. Run `npx gulp build` to rebuild assets.
3. Bump the version (see below) and commit.
4. Force-push: `git push --force-with-lease origin markstos`

## Versioning (Debian-style)

The version follows `<upstream-version>-markstos<N>`. Examples:

- Upstream is `1.0.0` → fork is `1.0.0-markstos1`, then `1.0.0-markstos2`, etc.
- If upstream bumps to `1.1.0` → reset suffix to `1.1.0-markstos1`.

Increment `N` on every rebase or fork-only change. Set the version in `package.json`.

## Commit conventions

Commit message format: `<version>: <short description>`

Example: `1.0.0-markstos2: rebase onto upstream/main, add translation support`

Include a co-author line:
```
Co-Authored-By: Oz <oz-agent@warp.dev>
```

## What makes this fork different from upstream

- `home.hbs`: highlights 7 most recent *featured* posts at the top of the page.
- `archive.hbs`: added archive page template.
- `assets/css/screen.css`: minor CSS customizations.
- `screenshots/`: fork-specific screenshots in `README.md`.
- `README.md`: updated with fork-specific documentation.
- `routes.yaml` example included.
- `yarn.lock` committed (upstream does not commit it).
- `.gitignore` committed (upstream does not have one).

## Translations

Upstream added `@tryghost/theme-translations` support. Base locale files live in
`locales/`. To override specific strings, create `locales-local/<lang>.json` with
only the keys you want to override — `npx gulp build` will merge them into `locales/`.

The theme name (`headline-markstos`) does not affect translation loading; Ghost
reads locale files from the active theme's `locales/` directory regardless of name.
