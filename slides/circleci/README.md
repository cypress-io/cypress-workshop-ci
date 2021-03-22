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
  cypress: cypress-io/cypress@1
# use cypress orb in your workflow
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
      - cypress/run
```

Commit and push the code.

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
### Build steps

![Circle build steps](./images/build-steps.png)

+++
### TODO: fix the build

Need to start the server and wait for the URL to respond

**💡 Hint:** look at the examples in [github.com/cypress-io/circleci-orb](https://github.com/cypress-io/circleci-orb)

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
```

---

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

+++
![Fast workflow](./images/fast.png)
