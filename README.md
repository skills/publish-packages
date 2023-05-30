<!--
  <<< Author notes: Header of the course >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses the MIT license.
-->

# Publish to GitHub Packages

_Use GitHub Actions to publish your project to a Docker image._

<!--
  <<< Author notes: Course start >>>
  Include start button, a note about Actions minutes,
  and tell the learner why they should take the course.
-->

## Welcome

GitHub Actions makes it easier than ever to incorporate continuous delivery (CD) into your repositories. This course will teach you what is needed to test and deliver artifacts that are ready for deployment.

- **Who is this for**: Developers, DevOps engineers, full stack developers, cloud engineers.
- **What you'll learn**: Continuous delivery, how to save and access build artifacts, package management, how to publish to GitHub Packages.
- **What you'll build**: We will build a Docker image that runs a small game.
- **Prerequisites**: We recommend you first complete the following courses: [Hello, GitHub Actions](https://github.com/skills/hello-github-actions) and [Continuous Integration](https://github.com/skills/continuous-integration).
- **How long**: This course takes less than 30 minutes to complete.

In this course, you will:

1.
2.
3.

### How to start this course

<!-- For start course, run in JavaScript:
'https://github.com/new?' + new URLSearchParams({
  template_owner: 'skills',
  template_name: 'publish-packages',
  owner: '@me',
  name: 'skills-publish-packages',
  description: 'My clone repository',
  visibility: 'public',
}).toString()
-->

[![start-course](https://user-images.githubusercontent.com/1221423/235727646-4a590299-ffe5-480d-8cd5-8194ea184546.svg)](https://github.com/new?template_owner=skills&template_name=publish-packages&owner=%40me&name=skills-publish-packages&description=My+clone+repository&visibility=public)

1. Right-click **Start course** and open the link in a new tab.
2. In the new tab, most of the prompts will automatically fill in for you.
   - For owner, choose your personal account or an organization to host the repository.
   - We recommend creating a public repository, as private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).
   - Scroll down and click the **Create repository** button at the bottom of the form.
3. After your new repository is created, wait about 20 seconds, then refresh the page. Follow the step-by-step instructions in the new repository's README.



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
   jobs:
     publish:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout
           uses: actions/checkout@v3
         # Add your test steps here if needed...
         - name: Docker meta
           id: meta
           uses: docker/metadata-action@v4
           with:
             images: ghcr.io/YOURNAME/publish-packages/game
             tags: type=sha
         - name: Login to GHCR
           uses: docker/login-action@v2
           with:
             registry: ghcr.io
             username: ${{ github.repository_owner }}
             password: ${{ secrets.GITHUB_TOKEN }}
         - name: Build container
           uses: docker/build-push-action@v4
           with:
             context: .
             push: true
             tags: ${{ steps.meta.outputs.tags }}
   ```
1. Replace `YOURNAME` with your username.
1. Make sure that the image name is unique.
1. Commit your changes.
1. (optional) Create a pull request to view all the changes you'll make throughout this course. Click the **Pull Requests** tab, click **New pull request**, set `base: main` and `compare:cd`.
1. Wait about 20 seconds then refresh this page for the next step.



<!--
  <<< Author notes: Step 2 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 2: Add a Dockerfile

_You created a publishing workflow! :tada:_

We will add a `Dockerfile` to the `cd` branch. The `Dockerfile` contains a set of instructions that get stored in a `Docker Image`. If you'd like, you can [learn more about Dockerfiles](https://docs.docker.com/engine/reference/builder/).

### :keyboard: Activity: Add a Dockerfile

1. In the `cd` branch, create `Dockerfile` at the project root and include:
   ```dockerfile
   FROM nginx:1.24-alpine
   COPY . /usr/share/nginx/html
   ```
1. Commit your changes.
1. Wait about 20 seconds then refresh this page for the next step.



<!--
  <<< Author notes: Step 3 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 3: Merge your changes

_Let's get publishing! :heart:_

You can now [merge](https://docs.github.com/en/get-started/quickstart/github-glossary#merge) your changes!

### :keyboard: Activity: Merge your changes

1. Merge your changes from `cd` into `main`. If you created the pull request in step 1, just open that PR and click on **Merge pull request**. If you did not create the pull request earlier, you can do it now by following the instructions in step 1.
1. (optional) Delete the branch `cd`.
1. Wait about 20 seconds then refresh this page for the next step.



<!--
  <<< Author notes: Step 4 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 4: Pull your image

_Now things are running! :sparkles:_

Whoa, now things are running! This may take a few minutes. This might take a tiny amount of time, so grab your popcorn :popcorn: and wait for the build to finish before moving on.

To pull the Docker image, we need to log into Docker first.

Before we can use this Docker image, you will need to generate a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) that contains the following permissions:

- repo (all)
- write:packages
- read:packages

![screenshot personal access token creation page with boxes for repo (all), write:packages, and read:packages checked](https://user-images.githubusercontent.com/3250463/219254714-82bb1da5-33b1-491b-97c0-b25f51494f6a.png)

We will use this token to log in to Docker, and authenticate with the package.

1. Open your terminal (Bash or Git Bash recommended).
1. Use the following command to log in:
    ```bash
    docker login ghcr.io -u USERNAME
    ```
1. Replace `USERNAME` with your GitHub username.
1. Enter your new Personal Access Token as the password.
1. Press **Enter**.

If everything went well, :crossed_fingers: you should see `Login Succeeded` in your terminal.

### :keyboard: Activity: Pull your image

1. Copy and paste the `pull` command from the package instructions into your terminal. It should look something like this:
   - `docker pull ghcr.io/YOURNAME/publish-packages/game:TAG`
   ![screenshot of the pull command on the GitHub package page](https://user-images.githubusercontent.com/3250463/219254981-9ff949fa-4d01-46e3-9e3d-b8ce3710c2a9.png)
   - _Tip: To reach this page, click the **Code** tab at the top of your repository. Then, find the navigation bar below the repository description, and click the **Packages** heading link_
1. Replace `YOURNAME` with your GitHub username.
1. Replace `TAG` with the image tag.
1. Press **Enter**.
1. You should see output indicating that the pull was successful, like `Status: Downloaded newer image for ghcr.io/YOURNAME/publish-packages/game:TAG`.
   ![screenshot of successful Docker image output](https://user-images.githubusercontent.com/3250463/219255178-3c943a6f-6c15-4f59-9002-228249b1c469.png)
1. _We can't automatically verify this step for you, so please continue on to the next step below!_



<!--
  <<< Author notes: Step 5 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 5: Run your image

_Nicely done grabbing your Docker image! :relaxed:_

Let's trying running it.

### :keyboard: Activity: Run your image

1. Find your image information by typing `docker image ls`.
   ![screenshot of output from Docker image ls command: lists docker images, REPOSITORY TAG and docker URL](https://i.imgur.com/UAwRXiq.png)<!-- This screenshot should be changed. -->
1. Use the following command to run a container from your image:
   ```bash
   docker run -dp 8080:80 --rm <YOUR_IMAGE_NAME:TAG>
   ```
1. Replace `YOUR_IMAGE_NAME` with your image name under the `REPOSITORY` column.
1. Replace `TAG` with the image tag under the `TAG` column.
1. Press **Enter**.
1. If everything went well, you will see hash value as output on your screen.
1. Optionally, you can open [localhost:8080](http://localhost:8080) to see the page you just created.
1. _We can't automatically verify this step for you, so please continue on to the next step below!_



<!--
  <<< Author notes: Finish >>>
  Review what we learned, ask for feedback, provide next steps.
-->

## Finish

_Congratulations friend, you've completed this course!_

<img src=https://octodex.github.com/images/collabocats.jpg alt=celebrate width=300 align=right>

Here's a recap of all the tasks you've accomplished in your repository:

- You wrote a workflow that sends a code through a continuous delivery pipeline.
- You built a fully deployable artifact.
- You did so using GitHub Actions and GitHub Packages!

### What's next?

- Publish your own packages from your projects.
- We'd love to hear what you thought of this course [in our discussion board](https://github.com/skills/.github/discussions).
- [Take another GitHub Skills course](https://github.com/skills).
- [Read the GitHub Getting Started docs](https://docs.github.com/en/get-started).
- To find projects to contribute to, check out [GitHub Explore](https://github.com/explore).



<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/skills/.github/discussions) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2023 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)
