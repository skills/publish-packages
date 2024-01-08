<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
-->

## Step 1: Create the workflow file

_Welcome to "Publish packages"! :wave:_

First, take a moment to examine the image below. It shows the relationship between _continuous integration_, _continuous delivery_ and _continuous deployment_.

![](https://i.imgur.com/xZCkjmU.png)

**Continuous integration** (CI) is a practice where developers integrate tested code into a shared branch several times per day. **Continuous delivery** (CD) is the next phase of **continuous integration** (CI), where we deploy our changes to the world.

[**Docker**](https://www.docker.com/why-docker) is an engine that allows you to run containers.
Containers are packages of software that can run reliably in different environments. Containers include everything needed to run the application. Containers are lightweight in comparison to virtual machines. A **Dockerfile** is a text document that contains all the commands and instructions necessary to build a Docker Image. A **Docker image** is an executable package comprised of code, dependencies, libraries, a runtime, environment variables, and configuration files. A **Docker container** is a runtime instance of a Docker Image.

We'll start by creating the workflow file to publish a Docker image to GitHub Packages.

### :keyboard: Activity: Create the workflow file

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Navigate to the **Code** tab.
1. From the **main** branch dropdown, click on the **cd** branch.
1. Navigate to the `.github/workflows/` folder, then select **Add file** and click on **Create new file**.
1. In the **Name your file...** field, enter `publish.yml`.
1. Add the following to the `publish.yml` file:
   ```yml
   name: Publish to Docker
   on:
     push:
       branches:
         - main
   permissions:
     packages: write
     contents: read
   jobs:
     publish:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout
           uses: actions/checkout@v4
         # Add your test steps here if needed...
         - name: Docker meta
           id: meta
           uses: docker/metadata-action@v5
           with:
             images: ghcr.io/YOURNAME/publish-packages/game
             tags: type=sha
         - name: Login to GHCR
           uses: docker/login-action@v3
           with:
             registry: ghcr.io
             username: ${{ github.repository_owner }}
             password: ${{ secrets.GITHUB_TOKEN }}
         - name: Build container
           uses: docker/build-push-action@v5
           with:
             context: .
             push: true
             tags: ${{ steps.meta.outputs.tags }}
   ```
1. Replace `YOURNAME` with your username.
1. Make sure that the image name is unique.
1. Commit your changes.
1. (optional) Create a pull request to view all the changes you'll make throughout this course. Click the **Pull Requests** tab, click **New pull request**, set `base: main` and `compare:cd`.
1. Wait about 20 seconds then refresh this page (the one you're following instructions from). [GitHub Actions](https://docs.github.com/en/actions) will automatically update to the next step.
