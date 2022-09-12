<!--
  <<< Author notes: Header of the course >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses Creative Commons Attribution 4.0 International.
-->

# Publish to GitHub Packages

_Use GitHub Actions to publish your project to a Docker image._

<!--
  <<< Author notes: Start of the course >>>
  Include start button, a note about Actions minutes,
  and tell the learner why they should take the course.
  Each step should be wrapped in <details>/<summary>, with an `id` set.
  The start <details> should have `open` as well.
  Do not use quotes on the <details> tag attributes.
-->

<!--step0-->

GitHub Actions makes it easier than ever to incorporate continuous delivery (CD) into your repositories. This course will teach you what is needed to test and deliver artifacts that are ready for deployment.

- **Who is this for**: Developers, DevOps engineers, full stack developers, cloud engineers.
- **What you'll learn**: Continuous delivery, how to save and access build artifacts, package management, how to publish to GitHub Packages.
- **What you'll build**: We will build a Docker image that runs a small game.
- **Prerequisites**: We recommend you first complete the following courses: [Hello, GitHub Actions](https://github.com/skills/hello-github-actions) and [Continuous Integration](https://github.com/skills/continuous-integration).
- **How long**: This course is five steps long and takes less than 30 minutes to complete.

## How to start this course

1. Above these instructions, right-click **Use this template** and open the link in a new tab.
   ![Use this template](https://user-images.githubusercontent.com/1221423/169618716-fb17528d-f332-4fc5-a11a-eaa23562665e.png)
2. In the new tab, follow the prompts to create a new repository.
   - For owner, choose your personal account or an organization to host the repository.
   - We recommend creating a public repository—private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).
   ![Create a new repository](https://user-images.githubusercontent.com/1221423/169618722-406dc508-add4-4074-83f0-c7a7ad87f6f3.png)
3. After your new repository is created, wait about 20 seconds, then refresh the page. Follow the step-by-step instructions in the new repository's README.

<!--endstep0-->

<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
-->

<details id=1>
<summary><h2>Step 1: Create the workflow file</h2></summary>

_Welcome to "Publish packages"! :wave:_

First, take a moment to examine the image below. It shows the relationship between _continuous integration_, _continuous delivery_ and _continuous deployment_.

![](https://i.imgur.com/xZCkjmU.png)

**Continuous integration** (CI) is a practice where developers integrate tested code into a shared branch several times per day. **Continuous delivery** (CD) is the next phase of **continuous integration** (CI), where we deploy our changes to the world.

[**Docker**](https://www.docker.com/why-docker) is an engine that allows you to run containers.
Containers are packages of software that can run reliably in different environments. Containers include everything needed to run the application. Containers are lightweight in comparison to virtual machines. A **dockerfile** is a text document that contains all the commands and instructions necessary to build a Docker Image. A **Docker image** is an executable package comprised of code, dependencies, libraries, a runtime, environment variables, and configuration files. A **Docker container** is a runtime instance of a Docker Image.

We'll start by creating the workflow file to publish a Docker image to GitHub Packages.

### :keyboard: Activity: Create the workflow file

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Open the pull request I made for you from the `cd` branch.
1. Add a new file at `.github/workflows/publish.yml`.
1. Add the following to the `publish.yml` file:
   ```yml
   name: Publish to Docker
   on:
     push:
       branches:
         - main
   jobs:
     publish:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         # Add your test steps here if needed...
         - name: Build container
           uses: docker/build-push-action@v1
           with:
             username: YOURNAME
             password: ${{ secrets.GITHUB_TOKEN }}
             registry: docker.pkg.github.com
             repository: YOURNAME/publish-packages/game
             tag_with_sha: true
   ```
1. Replace `YOURNAME` with your username.
1. Commit your changes.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 2 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

<details id=2>
<summary><h2>Step 2: Add a Dockerfile</h2></summary>

_You created a publishing workflow! :tada:_

We will add a `Dockerfile` in this pull request. The `Dockerfile` contains a set of instructions that get stored in a `Docker Image`. If you'd like, you can [learn more about Dockerfiles](https://docs.docker.com/engine/reference/builder/).

### :keyboard: Activity: Add a Dockerfile

1. In the `cd` branch, create `Dockerfile` at the project root and include:
   ```dockerfile
   FROM nginx:1.17
   COPY . /usr/share/nginx/html
   ```
1. Commit your changes.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 3 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

<details id=3>
<summary><h2>Step 3: Merge your pull request</h2></summary>

_Let's get publishing! :heart:_

You can now [merge](https://docs.github.com/en/get-started/quickstart/github-glossary#merge) your pull request!

### :keyboard: Activity: Merge your pull request

1. Click **Merge pull request**.
1. Delete the branch `cd` (optional).
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 4 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

<details id=4>
<summary><h2>Step 4: Pull your image</h2></summary>

_Now things are running! :sparkles:_

Whoa, now things are running! This may take a few minutes. This might take a tiny amount of time, so grab your popcorn :popcorn: and wait for the build to finish before moving on.

To pull the Docker image, we need to log into Docker first.

Before we can use this Docker image, you will need to generate a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) that contains the following permissions:

- repo (all)
- write:packages
- read:packages

![screenshot personal access token creation page with boxes for repo (all), write:packages, and read:packages checked](https://i.imgur.com/Ue9BJoV.png)

We will use this token to log in to Docker, and authenticate with the package.

1. Open your terminal (Bash or Git Bash recommended)
1. Use the following command to log in:
    ```
    docker login docker.pkg.github.com -u USERNAME -p TOKEN
    ```
1. Replace `USERNAME` with your GitHub username
1. Replace `TOKEN` with the Personal Access Token you just created
1. Press **Enter**

If everything went well, 🤞 you should see `Login Succeeded` in your terminal.

### :keyboard: Activity: Pull your image

1. Copy and paste the `pull` command from the package instructions into your terminal. It should look something like this:
   - `docker pull docker.pkg.github.com/YOURNAME/js-build/tic-tac-toe:f29`
   ![screenshot of the pull command on the GitHub package page](https://i.imgur.com/pFQgfSZ.png)
1. Press **Enter**.
1. You should see output indicating that the pull was successful, like `Status: Downloaded newer image for docker`.
   ![screenshot of successful Docker image output](https://i.imgur.com/i07kF2J.png)
1. _We can't automatically verify this step for you, so please continue on to the next step below!_

</details>

<!--
  <<< Author notes: Step 5 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

<details id=5>
<summary><h2>Step 5: Run your image</h2></summary>

_Nicely done grabbing your Docker image! :relaxed:_

Let's trying running it.

### :keyboard: Activity: Run your image

1. Find your image information by typing `Docker image ls`.
   ![screenshot of output from Docker image ls command: lists docker images, REPOSITORY TAG and docker URL](https://i.imgur.com/UAwRXiq.png)
1. Use the following command to run a container from your image:
   ```
   docker run -d -it --rm -p 8080:80 --name ttt <YOUR_IMAGE_NAME:TAG>
   ```
1. Replace `YOUR_IMAGE_NAME` with your image name under the `REPOSITORY` column.
1. Replace `TAG` with the image tag under the `TAG` column
   ![example of running the docker command listed above](https://i.imgur.com/hr6N9nk.png)
1. Press **Enter**.
1. If everything went well, you will see hash value as output on your screen.
1. _We can't automatically verify this step for you, so please continue on to the next step below!_

</details>

<!--
  <<< Author notes: Finish >>>
  Review what we learned, ask for feedback, provide next steps.
-->

<details id=X>
<summary><h2>Finish</h2></summary>

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

</details>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/skills/.github/discussions) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2022 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [CC-BY-4.0 License](https://creativecommons.org/licenses/by/4.0/legalcode)
