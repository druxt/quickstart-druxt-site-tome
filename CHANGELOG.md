# Changelog

All notable changes to this starterkit.

**The version tracks Druxt, which is still pre-1.0** (`druxt` 0.24.0,
`druxt-site` 0.14.3). A starterkit numbered above the framework it
builds on would claim a stability neither has, so this stays on 0.x
until Druxt reaches 1.0.

While it does, the usual 0.x reading applies:

- **Minor** - anything that changes the setup you would follow, up to
  and including a Drupal major. Breaking changes are called out under
  their own heading; on 0.x the minor is where they live.
- **Patch** - fixes and dependency updates that leave the documented
  setup alone.

## 0.3.0 (2026-08-21)

A full modernization: Drupal 11, a local backend that needs no Docker,
and a one-command install.

Breaking for anyone following the old setup, which on 0.x is what a
minor is for.

### Breaking changes

- Drupal 9 to **Drupal 11.4.5**, with druxt on 1.2, simple_oauth on 6.x
  and tome on 1.16, all D11-compatible releases.
- Provisioning installs with `drush site-install --existing-config`
  rather than `tome:install`. The committed config is the source of
  truth here, and a fresh install followed by a separate config import
  fails on a site UUID mismatch. A site built from the old template is
  unaffected; a fresh install follows the new path.
- The Gitpod configuration is gone, replaced by a dev container and CI.

### Features

- **One-command setup.** `npm install` on a fresh checkout provisions
  everything, which is what makes
  `npx giget gh:druxt/quickstart-druxt-site-tome my-site --install`
  deliver a running backend and frontend rather than an empty package.
  The same pipeline is available as `npm run setup`.
- **Tome content and files** are imported during provisioning, so a
  fresh checkout has the committed content without a database dump.
- **Test coverage**: end-to-end tests that provision a real backend and
  drive the built frontend, a test of the documented `giget` install
  path, guard-rail tests for machines without PHP, and container
  environment tests for DDEV, Lando and the dev container.
- **A Docker-free local backend** in `drupal/.devtools/`: Composer
  install, a site install, the OAuth consumer, and a PHP built-in
  server, driven by `assemble`, `provision`, `start`, `stop` and `info`.
  PHP and Composer are the only requirements.
- **Dev container** support for VS Code, Codespaces and DevPod, which
  sets the site up on create.
- **[Lando](https://lando.dev) as a backend option** alongside DDEV, with
  `lando drupal-install` and `lando druxt-add-consumer` running the same
  scripts the DDEV commands do.
- **Lifecycle commands** through npm, `make` and `mise`, including
  `npm run drush -- <command>` proxied to whichever backend is
  configured, and `npm run xdebug` to restart the backend with step
  debugging.
- **Windows guidance**: the local backend cannot run there, so setup says
  so immediately and names the routes that do work, instead of failing
  part way through key generation.
- **A lint suite** - ESLint, Prettier, cspell, markdownlint, knip,
  commitlint and Vale - so the starterkit holds itself to the practices
  it demonstrates.

### Bug fixes

- Druxt modules moved from `buildModules` to `modules`. `buildModules`
  are not loaded by `nuxt start`, so the proxy and authentication
  registrations vanished in production while the dev server looked fine.
- The OAuth consumer is created with the fields Simple OAuth 6 actually
  reads. It looks consumers up by `client_id` rather than uuid, and
  requires `grant_types`, but enforces both only through the entity
  form - so a programmatic save produced a consumer that could not
  authenticate, and login failed with `invalid_client`. The consumer is
  now validated before saving, and an OAuth2 scope is created and set as
  its default, because Simple OAuth 6 ships none and rejects every
  authorization request until one exists.
- `npm run dev` no longer fails on a missing export before it does
  anything. The guard it reaches for was never added to `lib.mjs`, so
  the error a reader saw was about a module rather than about their
  setup.
- Tome's subprocess commands are patched to build an argv array rather
  than a joined string.
- Setup runs one at a time. A dev container attaches while its
  post-create setup is still installing, and a second setup started from
  that terminal corrupted `vendor/` and `node_modules/`.
- `composer install` retries: a transient registry error no longer ends
  a first run.
- The dev container no longer leaves Xdebug active, which made every
  `php` and `composer` call wait for a debugger.
- The patch descriptions no longer link to a merge request that resolves
  only on a private network. `npm run lint:private` fails the build on
  any tracked file that references one.

### Dependencies

- Dependabot no longer files version updates. Renovate covers the same
  ecosystems and carries the auto-merge policy, so every bump was
  arriving twice. Dependabot security alerts are unaffected.

### Known limitations

- Nuxt 2 and Node 16 are both end of life. This starterkit is pinned to
  them because Druxt targets Nuxt 2; the Nuxt 3 story is separate work.
- `drupal/content/` carries users and shortcuts but no nodes, so a fresh
  checkout has nothing editorial to demonstrate Tome's export round-trip
  against.

## 0.2.0 (2022-09-29)

### Features

- Added a workflow to pull changes from the upstream quickstart.
- Added GitHub Actions, and `cweagans/composer-patches` for patched
  dependencies.

### Bug fixes

- Fixed the `drupal-install` command and the DDEV MTU problem on some
  networks.
- Pinned dependencies so builds stopped drifting.

## 0.1.0 (2022-02-08)

The first version of the starterkit: a Drupal 9 and Nuxt 2
mono-repo with Tome content sync, DDEV and Gitpod for local
development.
