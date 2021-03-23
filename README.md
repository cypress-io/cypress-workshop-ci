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

## Content index

1. Introduction [slides](https://cypress-workshop-ci.netlify.app/?p=intro)
   1. requirements
   2. example repo [cypress-io/cypress-workshop-ci-example](https://github.com/cypress-io/cypress-workshop-ci-example)
   3. NPM scripts and commands
   4. Cypress binary and info
2. Running Cypress on generic CI [slides](https://cypress-workshop-ci.netlify.app/?p=generic-ci)
   1. GitHub CI workflow
   2. caching dependencies
   3. waiting for the server to start
   4. storing test artifacts on CI
   5. recording tests on Cypress Dashboard
3. Cypress GitHub Action [slides](https://cypress-workshop-ci.netlify.app/?p=github-action)
   1. installing and running Cypress using action
   2. building the application
   3. action versions
   4. run tests in parallel
   5. split workflow into jobs
4. Cypress CircleCI Orb [slides](https://cypress-workshop-ci.netlify.app/?p=circleci)
   1. running the tests
   2. recording the test artifacts
   3. saving the workspace
   4. testing in parallel
5. Cypress Netlify plug [slides](https://cypress-workshop-ci.netlify.app/?p=netlify-build)
   1. deploy project on Netlify
   2. run E2E tests after deploy
   3. recording test results
   4. run E2E tests before build
   5. set up Cypress GitHub Integration
   6. set up GitHub status checks

## Example application

We will test the example application from the repo [cypress-io/cypress-workshop-ci-example](https://github.com/cypress-io/cypress-workshop-ci-example). You should fork that repo under your GitHub account and use with each CI provider.
