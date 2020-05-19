---
title: How to Git in the Hub
separator: <!--s-->
revealOptions:
    transition: 'slide'
controls: true
progress: true
slideNumber: true
---
<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

# How to Git in the Hub
<!-- .element: class="fragment fade-down" data-fragment-index="1" -->
### 1. Github Actions
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->
### 2. Github Packages
<!-- .element: class="fragment fade-left" data-fragment-index="3" -->
### 3. Github Pages
<!-- .element: class="fragment fade-right" data-fragment-index="4" -->
### 4. Github Tips & Tricks
<!-- .element: class="fragment fade-left" data-fragment-index="5" -->
### 5. Github Next Features
<!-- .element: class="fragment fade-right" data-fragment-index="6" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

# Github Actions

### 1. CI/CD
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->
### 2. Publish packages
<!-- .element: class="fragment fade-left" data-fragment-index="3" -->
### 3. Actions & Apps
<!-- .element: class="fragment fade-right" data-fragment-index="4" -->
### 4. Cron jobs
<!-- .element: class="fragment fade-left" data-fragment-index="5" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions

```
name: My First Action

on:
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 10
      - run: npm install
      - run: npm test
      - run: npm run build
```

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Trigger Build

```
on: [push, pull_request]
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - develop
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->
```
on:
  push:
    branches: #branches_ignore
      - 'releases/**'
    tags: #tags_ignore
      - v1
      - v1.*
      - v1.?
      - v1.[1-2]0
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="3" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Trigger Build

```
on:
  push:
    branches:    
    - 'releases/**'
    - '!releases/**-alpha'
```

```
on:
  push:
    paths-ignore:
    - 'docs/**'
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="5" -->
```
on:
  push:
    paths:
    - '**.js'
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="6" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Jobs

```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 10
        env:
         MAGIC_NUMBER: 420
      - run: npm install
      - run: npm test
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
   - run: |
      npm install
      npm test
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->
```
runs-on: [self-hosted, linux]
runs-on: windows-latest, windows-2019, ubuntu-latest, ubuntu-18.04, ubuntu-16.04, macos-latest, macos-10.15
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="3" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Jobs

```
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      output1: ${{ steps.step1.outputs.test }}
      output2: ${{ steps.step2.outputs.test }}
    steps:
    - id: step1
      run: echo "::set-output name=test::hello"
    - id: step2
      run: echo "::set-output name=test::world"
  job2:
    runs-on: ubuntu-latest
    needs: job1
    steps:
    - run: echo ${{needs.job1.outputs.output1}} ${{needs.job1.outputs.output2}}
```

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Artefacts

```
name: Build and Deploy
on:
  push:
    branches:
      - master
jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build-prod
      - name: Archive Production Artifact
        uses: actions/upload-artifact@master
        with:
          name: dist
          path: dist
  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Download Artifact
        uses: actions/download-artifact@master
        with:
          name: dist
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@master
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Jobs

```
steps:
  - name: Yes beee
    if: ${{ success() }}
  - name: Mryn
    if: ${{ failure() }}
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Job Matrix

```
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [ubuntu-16.04, ubuntu-18.04]
    node: [6, 8, 10]
steps:
  - uses: actions/setup-node@v1
    with:
      node-version: ${{ matrix.node }}
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
include:
    - node: 13
    os: ubuntu-18.04
    experimental: true
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Services

```
services:
  nginx:
    image: nginx
    # Map port 8080 on the Docker host to port 80 on the nginx container
    ports:
      - 8080:80
  redis:
    image: redis
    # Map TCP port 6379 on Docker host to a random free port on the Redis container
    ports:
      - 6379/tcp
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
${{ job.services.nginx.ports['8080'] }}
${{ job.services.redis.ports['6379'] }}
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Environments & Secrets

```
env:
  BUILD_PREFIX: "-dev"
  VERSION_PREFIX: "0.0."
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->
```
steps.buildnumber.outputs.build_number
steps.buildnumber.outputs.build_sha
steps.id
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->
```
- name: set tags to docker images
  run: |
    docker tag service ${{ secrets.AWS_ECR_REGISTRY }}/arxum-reader${{ env.BUILD_PREFIX }}${{ steps.buildnumber.outputs.build_number }}:latest
```
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="3" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Marketplace

- Marketplace jobs
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="1" -->

- Custom jobs - [local building](https://github.com/nektos/act)
<!-- .element: class="fragment fade-in-then-out" data-fragment-index="2" -->

* Apps <!-- .element: class="fragment fade-in-then-out" data-fragment-index="3" -->
  * Probot <!-- .element: class="fragment fade-in-then-out" data-fragment-index="4" -->
  * Delete merged branches <!-- .element: class="fragment fade-in-then-out" data-fragment-index="5" -->
  * WIP <!-- .element: class="fragment fade-in-then-out" data-fragment-index="6" -->
  * Code Quality <!-- .element: class="fragment fade-in-then-out" data-fragment-index="7" -->
  * Slack / Slack Notifications <!-- .element: class="fragment fade-in-then-out" data-fragment-index="8" -->
  * Dependency updater, img bot .... <!-- .element: class="fragment fade-in-then-out" data-fragment-index="9" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - CRON Jobs
```
name: Backup Firestore
on:
  schedule:
    - cron:  '0 0 * * *'
env:
  PROJECT_ID: YOUR-PROJECT
  BUCKET: gs://YOUR-BUCKET
jobs:
  backup:
    runs-on: ubuntu-latest
    steps:
    - uses: GoogleCloudPlatform/github-actions/setup-gcloud@master
      with:
        service_account_key: ${{ secrets.GCP_SA_KEY }}
        export_default_credentials: true
    - run: |
      gcloud info
      gcloud config set project $PROJECT_ID
      gcloud firestore export $BUCKET
```

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Actions - Examples

- gCloud Frontend
<!-- .element: class="fragment fade-right" data-fragment-index="1" -->
- gCloud Backend
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->
- Docker Image Push (dockerhub, gCloud, aws)
<!-- .element: class="fragment fade-right" data-fragment-index="3" -->
- Firebase Deploy
<!-- .element: class="fragment fade-right" data-fragment-index="4" -->
- AWS Fargate Task
<!-- .element: class="fragment fade-right" data-fragment-index="5" -->
- SSH
<!-- .element: class="fragment fade-right" data-fragment-index="6" -->
- Slack Notifications
<!-- .element: class="fragment fade-right" data-fragment-index="7" -->
- JSON manipulation
<!-- .element: class="fragment fade-right" data-fragment-index="8" -->

<!--s-->
<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Packages

```
"publishConfig": {
    "registry": "https://npm.pkg.github.com/@ivansstoyanov"
}
```
<!-- .element: class="fragment fade-right" data-fragment-index="1" -->
```
npm publish
```
<!-- .element: class="fragment fade-left" data-fragment-index="2" -->
```
// .npmrc
@ivansstoyanov:registry=https://npm.pkg.github.com
```
<!-- .element: class="fragment fade-right" data-fragment-index="3" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Packages - Deployment

```
name: Game Changer Package
on:
  release:
    types: [created]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: npm install
      - run: npm test

  publish-npm:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://registry.npmjs.org/
      - run: npm install
      - run: npm build
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.npm_token}}

    publish-gpr:
        needs: build
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: actions/setup-node@v1
            with:
            node-version: 12
            registry-url: https://npm.pkg.github.com/
            scope: '@your-github-username'
        - run: npm install
        - run: npm publish
            env:
            NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```
<!-- .element: class="fragment fade-right" data-fragment-index="3" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github Pages

##### Organization type - username.github.io
<!-- .element: class="fragment fade-down" data-fragment-index="1" -->
##### Project type - https://username.github.io/{{repository}}/
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github - Tips & Trics

- Fuzzy finder<!-- .element: class="fragment fade-down" data-fragment-index="1" --> - "<kbd>t</kbd>"
<!-- .element: class="fragment fade-down" data-fragment-index="1" -->
- Link to code snippets (hold shift) (add issue)
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->

```diff
10 PRINT "GITHUB IS COOL"
- 20 GOTO 11
+ 20 GOTO 10
```
<!-- .element: class="fragment fade-right" data-fragment-index="3" -->
<details>
<summary>Click here to see terminal history + debug info</summary>
<pre>
info here
</pre>
</details>
<!-- .element: class="fragment fade-right" data-fragment-index="4" -->

- git shortlog -sn  [git-extra](https://github.com/tj/git-extras/blob/master/Commands.md)
<!-- .element: class="fragment fade-right" data-fragment-index="5" -->
- [Dark Theme](https://github.com/StylishThemes/GitHub-Dark)
<!-- .element: class="fragment fade-right" data-fragment-index="6" -->
<p align="center">
<a target="_blank" rel="noopener noreferrer" href="https://github.com/Arxum/core-connector/workflows/Arxum%20Core%20Connector/badge.svg"><img src="https://github.com/Arxum/core-connector/workflows/Arxum%20Core%20Connector/badge.svg" alt=""></a>
</p>
<!-- .element: class="fragment fade-right" data-fragment-index="7" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

## Github - Next Features

### 1. Codespaces - [Link](https://github.com/features/codespaces/)
<!-- .element: class="fragment fade-down" data-fragment-index="1" -->
### 2. Security - [Link](https://github.com/features/security)
<!-- .element: class="fragment fade-right" data-fragment-index="2" -->

<!--s-->

<!-- .slide: data-background="https://raw.githubusercontent.com/ivansstoyanov/github/gh-pages/images/logo-dark.svg" data-background-size="12%" data-background-position="top 20px left 20px"-->

# Questions

<img src="https://github.com/ivansstoyanov/github/blob/gh-pages/images/george.gif?raw=true" />
