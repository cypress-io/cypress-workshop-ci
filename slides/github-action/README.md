# Cypress GitHub Action

## 📚 You will learn

- How to **easily** run Cypress on GitHub Actions
- How to run Cypress tests in parallel
- How to split build and test jobs

💡 see [github.com/cypress-io/github-action](https://github.com/cypress-io/github-action)

---
## Motivation

You don't need to fiddle with caching, installation, flags, recording, etc. Let us (Cypress authors) write the CI configuration code.

Repo [github.com/cypress-io/github-action](https://github.com/cypress-io/github-action)
+++
![Cypress GitHub Action on GitHub Marketplace](./images/marketplace.png)

---

```yml
# .github/workflow/ci.yml
name: ci
on: [push]
jobs:
  build-and-test:
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout 🛎
        uses: actions/checkout@v2

      - name: Run Cypress tests 🧪
        uses: cypress-io/github-action@v2
        with:
          record: true
          start: npm start
          wait-on: 'http://localhost:8080'
        env:
          # pass the record key as environment variable
          # during this CI step
          CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```
Use the above workflow and inspect the run

+++
## TODO: Look at the action examples

[github.com/cypress-io/github-action#examples](https://github.com/cypress-io/github-action#examples)

+++
## TODO: show cypress info

💡 Hint: use the build command parameter
+++
```yml
- name: Run Cypress tests 🧪
  uses: cypress-io/github-action@v2
  with:
    build: npx cypress info
    record: true
    start: npm start
    wait-on: 'http://localhost:8080'
    browser: firefox
  env:
    # pass the record key as environment variable
    # during this CI step
    CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```

---
## Run tests in parallel

- run tests faster by load balancing them across N machines
- use group names and tags
- parallelize each group separately

+++
### TODO: split spec into multiple specs

![Split into 4 specs](./images/split.png)

+++
### TODO: run a matrix of 3 machines

```yml
name: ci
on: [push]
jobs:
  build-and-test:
    runs-on: ubuntu-20.04
    strategy:
      fail-fast: false
      matrix:
        # run 3 copies of the current job in parallel
        containers: [1, 2, 3]
    steps:
      - name: Checkout 🛎
        uses: actions/checkout@v2

      - name: Run Cypress tests 🧪
        uses: cypress-io/github-action@v2
        with:
          record: true
          start: npm start
          wait-on: 'http://localhost:8080'
        env:
          # pass the record key as environment variable
          # during this CI step
          CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```

💡 see [cypress-io/github-action#parallel](https://github.com/cypress-io/github-action#parallel)

+++
### What do you see?

![Same 4 specs ran on every machine](./images/same-specs.png)

+++
### Turn on parallel mode

```diff
  uses: cypress-io/github-action@v2
  with:
    record: true
+   parallel: true
    start: npm start
    wait-on: 'http://localhost:8080'
```

**TODO:** inspect the output from each machine

+++
### Dashboard machines view

![Dashboard machines view](./images/machines.png)
