# Druxt Quickstart (Tome)

A fully decoupled starter-kit: **Drupal 11** (backend, JSON:API) + **Nuxt 2** (frontend, via [DruxtSite](https://druxtjs.org)), with content managed by [Tome](https://www.drupal.org/project/tome) - your content lives as git-tracked files, not just a database.

If you opened this in a dev container / DevPod, setup already ran automatically (`npm install` at the repository root provisions the whole stack: frontend deps, Composer, a local Drupal site installed straight from the committed config and Tome content, and OAuth for authenticated editing).

| Service        | URL                                 |
| -------------- | ----------------------------------- |
| Drupal backend | http://127.0.0.1:8888               |
| Nuxt frontend  | not started yet - see the next step |

If setup hasn't run yet (e.g. you cloned this manually), run `npm run setup` in a terminal first.
