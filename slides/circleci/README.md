# Cypress CircleCI Orb

## 📚 You will learn

- How to **easily** run Cypress on CircleCI
- How to run Cypress tests in parallel
- How to split build and test jobs

💡 see [github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

---
## Motivation

You don't need to fiddle with caching, installation, flags, recording, etc. Let us (Cypress authors) write the CI configuration code.

```yml
version: 2.1
orbs:
  # use cypress orb in your workflow
  cypress: cypress-io/cypress@1
workflows:
  build:
    jobs:
      - cypress/run:
          start: npm start
          wait-on: 'http://localhost:8080'
```

Repo [github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

---
### TODO: try running the simplest job

Create file `circle.yml` in the root of the repo

```yml
version: 2.1
orbs:
  cypress: cypress-io/cypress@1
workflows:
  build:
    jobs:
      # "cypress" is the name of the imported orb
      # "run" is the name of the job defined in Cypress orb
      - cypress/run:
          start: npm start
          wait-on: 'http://localhost:8080'
```

Commit and push the code.

`start`, `wait-on` parameters [github.com/cypress-io/circleci-orb#examples](https://github.com/cypress-io/circleci-orb#examples)

+++
### Create CircleCI project

![CircleCI project](./images/00-circle.png)
+++
### Use existing config file

![Use the existing config file](./images/01-circle.png)

+++
### Start building

![The CircleCI build is running](./images/02-circle.png)

+++
### TODO: look at CircleCI steps

find the:
- container info
- code check out
- caching
- installation
- starting the app
- running tests

---
### Workspace

Cypress Orb automatically passes all files from one job to another using Cypress _workspace_. But saving it takes time.

![Successful CircleCI job](./images/all-steps.png)

+++
### Todo: remove "persisting to workspace" step

We only have the single test job, and do not intend to run any more jobs using this workspace. Thus we can skip saving the workspace to save time.

**💡 Hint:** look at the examples in [github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

+++

```yml
version: 2.1
orbs:
  cypress: cypress-io/cypress@1
workflows:
  build:
    jobs:
      - cypress/run:
          start: npm start
          wait-on: 'http://localhost:8080'
          no-workspace: true
```

`no-workspace` parameter [github.com/cypress-io/circleci-orb#examples](https://github.com/cypress-io/circleci-orb#examples)

+++
![Fast workflow](./images/fast.png)

---
### Record the test results

- Look up the project's recording key at `https://dashboard.cypress.io/projects/<id>/settings`
- Add to the CircleCI project's settings

+++
![Set CYPRESS_RECORD_KEY variable](./images/key.png)

+++
### TODO: Set the project to record

Look up the record option at [github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

**Bonus:** add tag to separate the recorded runs from CircleCI from other CIs
+++

```yml
version: 2.1
orbs:
  cypress: cypress-io/cypress@1
workflows:
  build:
    jobs:
      # "cypress" is the name of the imported orb
      # "run" is the name of the job defined in Cypress orb
      - cypress/run:
          start: npm start
          wait-on: 'http://localhost:8080'
          no-workspace: true
          record: true
          tags: circleci
```

+++

![Dashboard run with tags](./images/tags.png)

---
## CircleCI Orb is a text macro expansion

Can be checked statically via CircleCI CLI [https://circleci.com/docs/2.0/local-cli/](https://circleci.com/docs/2.0/local-cli/)

```
$ circleci config validate circle.yml
Config file at circle.yml is valid.
```
+++

But if we tried to use `tag` instead of `tags`

```
$ circleci config validate circle.yml
Error: Error calling workflow: 'build'
Error calling job: 'cypress/run'
Unexpected argument(s): tag
```
+++

We can see the "effective" config after Orb has been expanded in the workflow

```
$ circleci config process circle.yml
# Orb 'cypress-io/cypress@1' resolved to 'cypress-io/cypress@1.27.0'
version: 2
jobs:
  cypress/run:
    docker:
    - image: cypress/base:10
    parallelism: 1
    environment:
    - CYPRESS_CACHE_FOLDER: ~/.cache/Cypress
    steps:
    - run:
        command: echo "Assuming dependencies were installed using cypress/install job"
    - attach_workspace:
        at: ~/
    - checkout
    - restore_cache:
        keys:
        - cache-{{ arch }}-{{ .Branch }}-{{ checksum "package.json" }}
    - run:
        name: Install
        working_directory: ''
        command: "if [[ ! -z \"\" ]]; then\n  echo \"Installing using custom command\"\n  echo \"\"\n  \nelif [ \"false\" = \"true\" ]; then\n  echo \"Installing using Yarn\"\n  yarn install --frozen-lockfile\nelif [ ! -e ./package-lock.json ]; then\n  echo \"The Cypress orb uses 'npm ci' to install 'node_modules', which requires a 'package-lock.json'.\"\n  echo \"A 'package-lock.json' file was not found. Please run 'npm install' in your project,\"\n  echo \"and commit 'package-lock.json' to your repo.\"\n  exit 1\nelse\n  echo \"Installing dependencies using NPM ci\"\n  npm ci\nfi\n"
    - run:
        name: Verify Cypress
        command: npx cypress verify
        working_directory: ''
    - save_cache:
        key: cache-{{ arch }}-{{ .Branch }}-{{ checksum "package.json" }}
        paths:
        - ~/.npm
        - ~/.cache
    - run:
        name: Start
        command: npm start
        background: true
        working_directory: ''
    - run:
        name: Wait-on http://localhost:8080
        command: npx wait-on http://localhost:8080
    - run:
        name: Run Cypress tests
        no_output_timeout: 10m
        command: |
          npx cypress run \
             \
             \
             \
             \
             --record \
               \
               \
               --tag 'circleci'  \
               \
        working_directory: ''
workflows:
  build:
    jobs:
    - cypress/run
  version: 2
```

---
## Parallel testing

```diff
  - cypress/run:
      start: npm start
      wait-on: 'http://localhost:8080'
      no-workspace: true
      record: true
      tags: circleci
+     parallel: true
+     parallelism: 2
```

[github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

+++

![Failed job](./images/failed.png)

+++
We need to install dependencies using 1 job, then run multiple test jobs using the same workspace. Cypress orb saves and loads the workspace between the jobs automatically

```yml
version: 2.1
orbs:
  cypress: cypress-io/cypress@1
workflows:
  build:
    jobs:
      - cypress/install
      - cypress/run:
          requires:
            - cypress/install
          start: npm start
          wait-on: 'http://localhost:8080'
          no-workspace: true
          record: true
          tags: circleci
          parallel: true
          parallelism: 2
```

+++
### Two jobs in the workflow

![Workflow with two jobs](./images/workflow.png)

+++
### Machines view

![Machines view on Cypress Dashboard](./images/machines.png)

+++
### Parallelization calculator

![How fast would tests be if we run them on N machines](./images/calculator.png)

+++
### TODO: Check if 4 machines really take 10 seconds

```diff
- parallelism: 2
+ parallelism: 4
```

+++
![Three machines joined in time](./images/only-3.png)

---
## TODO

- build app (once) after installing dependencies
- run tests in several groups
- print Cypress info
- run tests using Chrome or Firefox browser
- store test artifacts on CircleCI
- run tests on Windows, or Mac

---
## Learn more

- "Start CircleCI Machines Faster by Using RAM Disk" at https://glebbahmutov.com/blog/circle-ram-disk/ Jan 2021
- "Make Cypress Run Faster by Splitting Specs" at https://glebbahmutov.com/blog/split-spec/ Dec 2020
- "Faster, easier, end-to-end testing with CircleCI and Cypress" at https://www.youtube.com/watch?v=v7FCj2LOWgE Oct 2020

---
## ⌛️ Review

- using [cypress-io/circleci-orb](github.com/cypress-io/circleci-orb) is the simplest way to install, cache, and run Cypress tests on CircleCI
- running tests in parallel is as simple as `parallelism: N`
- the orb passes the files through the workspace automatically

Jump to: [Generic CI](/?p=generic-ci), [GitHub Action](/?p=github-action), [CircleCI](/?p=circleci), [Netlify Build](/?p=netlify-build)
