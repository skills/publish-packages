<!--
  <<< Author notes: Header of the course >>>
  Include a 1280×640 image, course title in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Add your open source license, GitHub uses Creative Commons Attribution 4.0 International.
-->

<img src=https://repository-images.githubusercontent.com/216605017/627c7780-57db-11ea-990b-17c6ffdff523 width=300 align=right>

# GitHub Actions: Publish to GitHub Packages

_This course will walk you through using GitHub Actions to get your code in a deployable state once your CI workflows have completed._

<!--
  <<< Author notes: Start of the course >>>
  Include start button, a note about actions minutes,
  and tell the learner why they should take the course.
  Each step should be wrapped in <details>/<summary>, with an `id` set.
  The start <details> should have `open` as well.
  Do not use quotes on the <details> tag attributes.
-->

<details id=0 open>
<summary><strong>:golf: Start</strong></summary>

**To start this course: [<img width="150" alt="Use this template" src="https://user-images.githubusercontent.com/1221423/148581131-555c0fb8-5361-4450-a760-75fa6219a2fc.png">](https://github.com/githublearn/publish-packages/generate)**

> We recommend creating a public repository, as private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).<br>
> After you make your own repository, wait about 20 seconds and refresh. I will go to the next step.

GitHub Actions makes it easier than ever to incorporate continuous delivery (CD) into your repositories. This course will teach you what is needed to test and deliver artifacts that are ready for deployment.

- **Who is this for**: Developers, DevOps engineers, full stack developers, cloud engineers.
- **What you'll learn**: Continuous delivery, save and access build artifacts, package management, how to publish to GitHub Packages.
- **What you'll build**: We will build a Docker image that runs a small game.
- **Prerequisites**: We recommend you first complete the following courses: [Hello, GitHub Actions](https://github.com/githublearn/hello-github-actions) and [Continuous Integration](https://lab.github.com/githublearn/continuous-integration).
- **How long**: This course is 5 steps and takes less than 30 minutes.

</details>

<!--
  <<< Author notes: Step 1 >>>
  Choose 3-5 steps for your course.
  The first step is always the hardest, so pick something easy!
  Link to docs.github.com for further explanations.
  Encourage users to open new tabs for steps!
-->

<details id=1>
<summary><strong>:pencil: Step 1: Create the workflow file</strong></summary>

### :wave: Welcome to "Publish packages"!

First, take a moment to examine the image below. It shows the relationship between _continuous integration_, _continuous delivery_ and _continuous deployment_.

![](https://i.imgur.com/xZCkjmU.png)

**Continuous integration** (CI) is a practice where developers integrate tested code into a shared branch several times per day. **Continuous delivery** (CD) is the next phase of **continuous integration** (CI), where we deploy our changes to the world.

[**Docker**](https://www.docker.com/why-docker) is an engine that allows you to run containers.
Containers are packages of software that can run reliably in different environments. Containers include everything needed to run the application. Containers are lightweight in comparison to virtual machines. A **dockerfile** is a text document that contains all the commands and instructions necessary to build a Docker Image. A **Docker image** is an executable package comprised of code, dependancies, libraries, a runtime, environment variables, and configuration files. A **Docker container** is a runtime instance of a Docker Image.

We'll start by creating the workflow file to publish a Docker image to GitHub Packages.

### :keyboard: Activity: Create the workflow file

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Open the pull request I made for you from the `cd` branch.
1. Add a new file at `.github/workflows/publish.yml`.
1. Add the following to the `publish.yml` file:
   ```yml
   TBD
   ```
1. Commit your changes.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 2 >>>
  Start this step by acknowledging the first step.
  Define terms and link to docs.github.com.
-->

<details id=2>
<summary><strong>:ship: Step 2: Add a Dockerfile</strong></summary>

### :tada: You created a publishing workflow!

We will add a `Dockerfile` in this pull request. The `Dockerfile` contains a set of instructions that get stored in a `Docker Image`. If you'd like, you can [learn more about Dockerfiles](https://docs.docker.com/engine/reference/builder/).

### :keyboard: Activity: Add a Dockerfile

1. In the `cd` branch, edit the `Dockerfile` to include:
   ```dockerfile
   FROM nginx:1.17
   COPY . /usr/share/nginx/html
   ```
1. Commit your changes.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 3 >>>
  Start this step by acknowledging the first step.
  Define terms and link to docs.github.com.
-->

<details id=3>
<summary><strong>:shipit: Step 3: Merge your pull request</strong></summary>

### :heart: Let's get publishing!

You can now [merge](https://help.github.com/articles/github-glossary/#merge) your pull request!

### :keyboard: Activity: Merge your pull request

1. Click **Merge pull request**.
1. Delete the branch `cd` (optional).
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 4 >>>
  Start this step by acknowledging the first step.
  Define terms and link to docs.github.com.
-->

<details id=4>
<summary><strong>:rowboat: Step 4: Pull your image</strong></summary>

### Nice work TBD :sparkles:

TBD

**What is _TBD_**: TBD

### :keyboard: Activity: TBD

1. TBD.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Step 5 >>>
  Start this step by acknowledging the first step.
  Define terms and link to docs.github.com.
-->

<details id=5>
<summary><strong>:runner: Step 5: Run your image</strong></summary>

### Nicely done friend! :sparkles:

TBD

**What is _TBD_**: TBD

### :keyboard: Activity: TBD

1. TBD.
1. Wait about 20 seconds then refresh this page for the next step.

</details>

<!--
  <<< Author notes: Finish >>>
  Review what we learned, ask for feedback, provide next steps.
-->

<details id=X>
<summary><strong>:checkered_flag: Finish</strong></summary>

### Congratulations friend, you've completed this course!

<img src=https://octodex.github.com/images/collabocats.jpg alt=celebrate width=300 align=right>

Here's a recap of all the tasks you've accomplished in your repository:

- You wrote a workflow that sends a code through a continuous delivery pipeline.
- You built a fully deployable artifact.
- You did so using GitHub Actions and GitHub Packages!

### What's next?

- Publish your own packages from your projects.
- We'd love to hear what you thought of this course [in our community forum](https://github.community/c/education/github-learning-lab/34).
- [Take another GitHub Learn Course](https://github.com/githublearn).
- [Read the GitHub Getting Started docs](https://docs.github.com/en/get-started).
- To find projects to contribute to, check out [GitHub Explore](https://github.com/explore).

</details>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our community forum](https://github.community/c/education/github-learning-lab/34) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2022 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [CC-BY-4.0 License](https://creativecommons.org/licenses/by/4.0/legalcode)
