# Simple Node CI

Simple opinionated node pipeline.

Steps:
- Git checkout
- Install dependencies
- Run CodeQL analysis
- Run lint
- Run build
- Run unit tests
- Run integrations tests (Playwright, Cypress, custom)
  - For Playwright and Cypress, browsers are automatically downloaded
- Upload integration test result

If you want to add a simple CI pipeline to your Node/JS/TS project, just add a workflow yaml to your project as follows:

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

permissions:
  actions: read
  contents: read
  security-events: write

jobs:
  ci:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
    - uses: andrehil/simple-node-ci@main
      with:
        node-version: lts/*
```

Parameters:
  - **node-version**: (required) Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.
  - **cache**: (default: 'npm') A package manager for caching in the default directory. Supported values: npm, yarn, pnpm.'
  - **cache-dependency-path**: The path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.
  - **install-command**: (default: 'npm ci') The command that will be executed to install the dependencies.
  - **lint-command**: (default: 'npm run lint') The command that will be executed to lint on the project.
  - **build-command**: (default: 'npm run build') The command that will be executed to build the project.
  - **test-command**: (default: 'npm test') The command that will be executed to run unit tests.
  - **start-command**: (default: 'npm start') The command that will be executed to start the application.
  - **integration-test-lib**: (default: 'playwright') The library used to run integration tests. Accepts "playwright", "cypress", or "" for custom tests.
  - **integration-test-command**: (default: 'npm run test:pw') The command that will be executed to run integration tests.
