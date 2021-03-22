# cypress-workshop-ci
> A workshop that teaches you how to run Cypress on major CI providers

**Warning: ⚠️** this is not an introduction to Cypress testing workshop, only how to successfully run Cypress on CI. If you need to refresh your Cypress skills, check out the [testing-workshop-cypress](https://github.com/cypress-io/testing-workshop-cypress)

## Requirements

You will need:

- `git` to clone the repository
- Node v12+ to install and run locally
- 👉 fork the [cypress-io/cypress-workshop-ci-example](https://github.com/cypress-io/cypress-workshop-ci-example) now 👈

### Create free accounts

- GitHub [https://github.com/](https://github.com/)
- CircleCI [https://circleci.com/](https://circleci.com/)
- Netlify [https://www.netlify.com/](https://www.netlify.com/)
- Cypress Dashboard [https://dashboard.cypress.io/](https://dashboard.cypress.io/)

## Slides 🖥

See the presentation at [https://cypress-workshop-ci.netlify.app/][presentation]. Every section of the presentation has a subfolder in the [slides](./slides) folder with a Markdown file. The Markdown is rendered into HTML using [Vite and Reveal.js combination](https://glebbahmutov.com/blog/reveal-vite/). You can open the presentation by clicking on "link" in the table below.

[presentation]: https://cypress-workshop-ci.netlify.app/

## Content 🗂

topic | Markdown | view slides
---|---|---
Introduction | [intro](./slides/intro/README.md) | [intro slides](https://cypress-workshop-ci.netlify.app/?p=intro)
Generic CI | [generic-ci](./slides/generic-ci/README.md) | [generic ci slides](https://cypress-workshop-ci.netlify.app/?p=generic-ci)
GitHub Action | [github-action](./slides/github-action/README.md) | [github action slides](https://cypress-workshop-ci.netlify.app/?p=github-action)
Circle CI Orb | [circleci](./slides/circleci/README.md) | [circleci slides](https://cypress-workshop-ci.netlify.app/?p=circleci)
Netlify Build plugin | [netlify-build](./slides/netlify-build/README.md) | [netlify build slides](https://cypress-workshop-ci.netlify.app/?p=netlify-build)

## Example application

We will test the example application from the repo [cypress-io/cypress-workshop-ci-example](https://github.com/cypress-io/cypress-workshop-ci-example). You should fork that repo under your GitHub account and use with each CI provider.
