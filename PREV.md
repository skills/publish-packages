














---

# Welcome

Welcome to the Learning Lab course "Using GitHub Actions for CD". What is **CD**? If you don't already know, you'll find out soon! 

Before you get started, it's important that you are comfortable with a few skills. We recommend going through the [Introduction to GitHub Learning Lab course](https://lab.github.com/githubtraining/introduction-to-github/) and the [Hello, GitHub Actions! Learning Lab course](https://lab.github.com/github/hello-github-actions!) before beginning this one. 

Other experience with workflow automation will help, but are not necessary to complete this course.

### Set up CI Workflow

#### What is CI?

First, take a moment to examine the image below. It shows the relationship between **continuous integration**, **continuous delivery** and **continuous deployment**.

![](https://i.imgur.com/xZCkjmU.png)

**Continuous integration** (CI) is a practice where developers  integrate code into a shared branch several times per day.  The shared branch is sometimes referred to as **trunk**, but on Git, it's named **main**. To integrate code, developers **commit** on other Git branches, **push** their changes, and **merge** to main through **pull requests**. 

Automated events take place throughout this process. These events can range from running tests or deployments to cross-linking to relevant threads. Here's an example that we will use:

- Source code goes through an automated build process if necessary
- Automated testing of the software takes place
- Reports are generated and sent back to the developers with the status of their changes

**GOAL:** Regular code integration enables faster and easier error detection.

### Why do we need this?

**Continuous delivery** (CD) is the natural "next phase" of **continuous integration** (CI). Setting up a CI workflow will show us the entire picture of our workflow.

But, this is **NOT** a course on CI. We will not being going into detail on what CI means, or how to use CI with GitHub Actions.

Wait! There's good news 👍! If you need a CI refresher you can take the [Using GitHub Actions for CI Learning Lab course](https://lab.github.com/githubtraining/github-actions:-continuous-integration) to get up to speed.


# Moving the workflow file to `.github/workflows/ci-workflow.yml`

There is a file on this branch called `ci-workflow.yml`. But, now I need your help!

## Step 1: Move the workflow file

As a bot 🤖, I can only do so many things. Setting up a workflow for you isn't something I can do.  Don't worry, I'll be here to help you, just follow these steps and we should be good to go. 👍

Please move the `ci-workflow.yml` file into a new folder and update the contents of the file.

At first, it will look like this: 
![screenshot of file title as `.github/ci-workflow.yml](https://i.imgur.com/xPwoHGc.png) 

When you're done renaming, you will have created a new directory and moved the file into that new directory. Your file title should look like this:

![screenshot of file title as `.github/workflows/ci-workflow.yml](https://i.imgur.com/6QLdM56.png)

### :keyboard: Move the `ci-workflow.yml` file

1. [Edit the file location]({{ repoUrl }}/edit/ci-workflow/.github/ci-workflow.yml) by changing its name from `ci-workflow.yml` to `workflows/ci-workflow.yml`.
2. Commit the file to the `ci-workflow` branch once you have changed the path:
![screenshot highlighting the commit changes button](https://i.imgur.com/fJPdNqj.png)

# Oops!

It looks like you didn't name the workflow file properly.  

Be sure the path is `.github/workflows` and the filename is `ci-workflow.yml`. **I'll be waiting for a new commit on this branch.**

![screenshot of updated title to .github/workflows/ci-workflow.yml](https://i.imgur.com/m1MM9Zo.png)


# Docker 🐳

### What is Docker?

Docker is an engine that allows you to run containers. Containers have many advantages, including:

- Code and dependancies are packaged together, so software runs more reliably in different environments
- Containers are a standard unit of software
- As standalone executable packages, containers include everything needed to run the application
- Containers are lightweight in comparison to virtual machines

---

#### Docker vs Virtual Machines

![visualization comparing containers to virtual machines](https://i.imgur.com/WnAsfyI.png)

| Container                                                       | Virtual Machine                                                                          |
| --------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Application layer abstraction                                   | Physical layer abstraction                                                               |
| Consume less space than VMs                                     | Include a full copy of the operating system                                              |
| Fast to startup                                                 | Boot up slowly                                                                           |
| Shared OS kernel allows many containers to run on a single host | Multiple virtual machines run on one server at a greater resource cost per guest machine |

#### Dockerfiles, Images, and Container

Before moving forward with the workflow file, let's spend some time on these concepts. There are important differences between *Dockerfiles*, *Images*, and *Containers*.

| Dockerfile                                                                                      | Docker Image                                                                                                                                                         | Docker Container                                                                                     |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Text document that contains all the commands and instructions necessary to build a Docker Image. | Executable packages comprised of code, dependancies, libraries, a runtime, environment variables, and configuration files.  Very similar to a virtual machine snapshot. | A runtime instance of a Docker Image.  This is what the image becomes when executed in memory. |

---

#### What about our workflow?

Our repository contains a `Dockerfile`, source code, and tests for the Tic Tac Toe application.

Our CI Workflow allows us to make code changes. Those changes will trigger an automated build and automated test. But, the automation does not create a deployable artifact.

We will place our application into a Docker container. Then, we will have a _deployable package_.  A deployable package enables CD.

Because a `Dockerfile` is a text file, we are able to version it as source code. This _configuration as code_ allowing us a single point of truth for our application.

As you learned above, we need to turn that _Dockerfile_ into a _Docker image_ if we want to create a runtime instance. We are going to store that image in _GitHub Packages_.

---

📖[Read More](https://www.docker.com/why-docker) about Docker.


# Edit the workflow file

## Step 2: Edit the workflow file

We are going to edit the current workflow file in our repository by adding a job that turns our `Dockerfile` into a `Docker Image`.

### :keyboard: Activity: Edit the workflow file and turn the Dockerfile into a Docker Image

1. [Edit the current workflow file in our repository]({{ repoUrl }}/blob/docker-workflow/.github/workflows/ci-workflow.yml)
2. Rename `ci-workflow.yml` to `cd-workflow.yml`:
3. On line 1, change the name from `Node CI` to `Docker CD`
   `yaml name: Docker CD`
4. Add the following job to your workflow file:

```yaml
  Build-and-Push-Docker-Image:
    runs-on: ubuntu-latest
    needs: test
    name: Docker Build, Tag, Push

    steps:
    - name: Checkout
      uses: actions/checkout@v1
    - name: Download built artifact
      uses: actions/download-artifact@main
      with:
        name: webpack artifacts
        path: public
    - name: Build container image
      uses: docker/build-push-action@v1
      with:
        username: {% raw %}${{github.actor}}{% endraw %}
        password: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
        registry: docker.pkg.github.com
        repository: {{ user.login }}/github-actions-for-packages/tic-tac-toe
        tag_with_sha: true
```

5. Commit the newly edited workflow file to your repository

<details><summary>Complete <b>cd-workflow.yml</b> file...</summary>

```yaml
name: Docker CD

on:
  push:
    paths:
    - "**Dockerfile**"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: npm install and build webpack
      run: |
        npm install
        npm run build
    - uses: actions/upload-artifact@main
      with:
        name: webpack artifacts
        path: public/

  test:
    runs-on: ubuntu-latest
    needs: build
    strategy:
      matrix:
        os: [ubuntu-lastest, windows-2016]
        node-version: [12.x, 14.x]

    steps:
    - uses: actions/checkout@v1
    - name: Use Node.js {% raw %}${{ matrix.node-version }}{% endraw %}
      uses: actions/setup-node@v1
      with:
        node-version: {% raw %}${{ matrix.node-version }}{% endraw %}
    - uses: actions/download-artifact@main
      with:
        name: webpack artifacts
        path: public
    - name: npm install, and test
      run: |
        npm install
        npm test
      env:
        CI: true

  Build-and-Push-Docker-Image:
    runs-on: ubuntu-latest
    needs: test
    name: Docker Build, Tag, Push

    steps:
    - name: Checkout
      uses: actions/checkout@v1
    - name: Download built artifact
      uses: actions/download-artifact@main
      with:
        name: webpack artifacts
        path: public
    - name: Build container image
      uses: docker/build-push-action@v1
      with:
        username: {% raw %}${{github.actor}}{% endraw %}
        password: {% raw %}${{secrets.GITHUB_TOKEN}}{% endraw %}
        registry: docker.pkg.github.com
        repository: {{ user.login }}/github-actions-for-packages/tic-tac-toe
        tag_with_sha: true
```

</details>


### Great Job 👍

I have merged this pull request for you, and opened a new one for you to start working on the CD segment of our workflow.

Navigate [to the next pull request]({{ pullURL }}) to continue this course.


# Oops!

It looks like you didn't set up your workflow file properly.  

Be sure the path is `.github/workflows` and the filename is `cd-workflow.yml`. **I'll be waiting for a new commit on this branch.**

![screenshot showing path as .github/workflows/cd-workflow.yml](https://user-images.githubusercontent.com/16547949/67888662-ea9b8a80-fb23-11e9-8cb2-d751a0e483cc.png)


# Creating a Dockerfile 🐳

You may have noticed that your workflow didn't run as expected. That's because we don't have a `Dockerfile` in our repository yet!

We will add a `Dockerfile` in this pull request. I'll leave a comment with instructions to help you. 😄

# Dockerfile 🐳

The `Dockerfile` contains a set of instructions that get stored in a `Docker Image`.

We will use a very simple `Dockerfile`. If you'd like, you can [learn more about Dockerfiles](https://docs.docker.com/engine/reference/builder/).

## Step 3: Add a Dockerfile image

### :keyboard: Activity: Edit the Dockerfile to include the proper image

1. [Edit the `Dockerfile`]({{ repoUrl }}/blob/add-dockerfile/Dockerfile) located on the `add-dockerfile` branch in the root of the repository
2. Paste the following contents inside of the Docker file:

```dockerfile
FROM nginx:1.17
COPY . /usr/share/nginx/html
```

3. Commit your changes to the `add-dockerfile` branch


### Great Job 👍

I have merged this pull request for you, and opened a new one for you to start working on the CD segment of our workflow.

Navigate [to the next pull request]({{ pullURL }}) to continue this course.


# Now things are running!

Whoa, now things are running! This may take a few minutes.

After committing the `Dockerfile`, the repository had the components it needed to start the CD workflow.

This might take a tiny amount of time, so grab your popcorn 🍿 and wait. I'll respond one your pipeline has finished running, until then... sit tight!

_You can click the `Actions` tab to view your workflow in progress. Make sure you come back to this pull request._


# Oh oh...

Something went wrong in your workflow.

Navigate to the [Actions tab]({{ repoUrl }}/actions) to troubleshoot. Recommit the necessary fixes to this repository.

# Using your new Docker Image package

The easiest way to use your Docker Image package is to click the package name from the [package tab]({{ repoUrl }}/packages).
![screenshot of packages, highlighting the tic-tac-toe title](https://i.imgur.com/d2nU7gj.png)

That will bring you to a screen like containing instructions.
![screenshot of tic-tac-toe package page](https://i.imgur.com/lkj2Ggy.png)

---

**Before we can use this Docker image, you will need to generate a [personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) that contains the following permissions:**
- repo (all)
- write:packages
- read:packages

![screenshot personal access token creation page with boxes for repo (all), write:packages, and read:packages checked](https://i.imgur.com/Ue9BJoV.png)

We will use this token to log in to Docker, and authenticate with the package.

## Step 4: Log in to Docker

### :keyboard: Activity: Log in to Docker

1. Open your terminal (Bash or Git Bash recommended)
2. Use the following command to log in:
    ```
    docker login docker.pkg.github.com -u USERNAME -p TOKEN
    ```
3. Replace `USERNAME` with your GitHub username
4. Replace `TOKEN` with the Personal Access Token you just created
5. Press **Enter**

If everything went well, 🤞 you should see `Login Succeeded` in your terminal.


## Step 5: Pull your image

### :keyboard: Pull the image from GitHub Packages to your local environment

1. Copy and paste the `pull` command from the package instructions into your terminal. It should look something like this:
  - `docker pull docker.pkg.github.com/{{ user.username }}/js-build/tic-tac-toe:f29`
![screenshot of the pull command on the GitHub package page](https://i.imgur.com/pFQgfSZ.png)
2. Press **Enter**

You should see output indicating that the pull was successful, like `Status: Downloaded newer image for docker.`

![screenshot of successful Docker image output](https://i.imgur.com/i07kF2J.png)

## Step 6: Run your Docker Image

### :keyboard: Activity: Run your image locally

1. Find your image information by typing `Docker image ls`
        ![screenshot of output from Docker image ls command: lists docker images, REPOSITORY TAG and docker URL](https://i.imgur.com/UAwRXiq.png)
2. Use the following command to run a container from your image:
        ```
        docker run -d -it --rm -p 8080:80 --name ttt <YOUR_IMAGE_NAME:TAG>
        ```
3. Replace `YOUR_IMAGE_NAME` with your image name under the `REPOSITORY` column
4. Replace `TAG` with the image tag under the `TAG` column
        ![example of running the docker command listed above](https://i.imgur.com/hr6N9nk.png)
5. Press **Enter**

If everything went well, you will see hash value as output on your screen.

# Congratulations! 🎉

Congratulations @{{ user.username }}, you did it!  You wrote a workflow that sends a code through a CI/CD pipeline and leaves you will a fully tested, fully deployable artifact.

You did so using GitHub Actions and GitHub Packages!

This concludes our course.  Close this issue when you are done.  You are free to keep this repository for future reference as you go out and build cool things on the internet!

![celebrate](https://octodex.github.com/images/collabocats.jpg)

## What's next?

We hope this course helped you feel more comfortable using GitHub. Remember: You’re always welcome to repeat course topics if you’d like additional practice. If you’re ready to build on your new skills, here are some ideas.

### Contribute to a project

To find projects to contribute to through trending topics and curated collections, check out [GitHub Explore](https://github.com/explore).

### Keep learning

[What will you learn next]({{ host }}/courses)?

# You did it!

It looks like your workflow run was successful. Awesome!

You now have a Docker image stored in the GitHub Packages.

![screenshot of repository navigation highlighting the "1 package" button](https://i.imgur.com/HWwlmtn.png)

**You may need to switch to the `main` branch to see your package count increase.**

![screenshot of the packages built as seen at the top of the `code` tab](https://i.imgur.com/e9zrFGf.png)

You should now see a package named `tic-tac-toe`. 🎉

---

I'll go ahead and merge this pull request. [I'll give you the next steps in the next issue]({{ nextUrl }}).

# Using your new Docker Image package

The easiest way to use your Docker Image package is to click the package name from the [package tab]({{ repoUrl }}/packages).
![screenshot of packages, highlighting the tic-tac-toe title](https://i.imgur.com/d2nU7gj.png)

That will bring you to a screen like containing instructions.
![screenshot of tic-tac-toe package page](https://i.imgur.com/lkj2Ggy.png)

---

**Before we can use this Docker image, you will need to generate a [personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) that contains the following permissions:**
- repo (all)
- write:packages
- read:packages

![screenshot personal access token creation page with boxes for repo (all), write:packages, and read:packages checked](https://i.imgur.com/Ue9BJoV.png)

We will use this token to log in to Docker, and authenticate with the package.

## Step 4: Log in to Docker

### :keyboard: Activity: Log in to Docker

1. Open your terminal (Bash or Git Bash recommended)
2. Use the following command to log in:
    ```
    docker login docker.pkg.github.com -u USERNAME -p TOKEN
    ```
3. Replace `USERNAME` with your GitHub username
4. Replace `TOKEN` with the Personal Access Token you just created
5. Press **Enter**

If everything went well, 🤞 you should see `Login Succeeded` in your terminal.


## Step 5: Pull your image

### :keyboard: Pull the image from GitHub Packages to your local environment

1. Copy and paste the `pull` command from the package instructions into your terminal. It should look something like this:
  - `docker pull docker.pkg.github.com/{{ user.username }}/js-build/tic-tac-toe:f29`
![screenshot of the pull command on the GitHub package page](https://i.imgur.com/pFQgfSZ.png)
2. Press **Enter**

You should see output indicating that the pull was successful, like `Status: Downloaded newer image for docker.`

![screenshot of successful Docker image output](https://i.imgur.com/i07kF2J.png)

## Step 6: Run your Docker Image

### :keyboard: Activity: Run your image locally

1. Find your image information by typing `Docker image ls`
        ![screenshot of output from Docker image ls command: lists docker images, REPOSITORY TAG and docker URL](https://i.imgur.com/UAwRXiq.png)
2. Use the following command to run a container from your image:
        ```
        docker run -d -it --rm -p 8080:80 --name ttt <YOUR_IMAGE_NAME:TAG>
        ```
3. Replace `YOUR_IMAGE_NAME` with your image name under the `REPOSITORY` column
4. Replace `TAG` with the image tag under the `TAG` column
        ![example of running the docker command listed above](https://i.imgur.com/hr6N9nk.png)
5. Press **Enter**

If everything went well, you will see hash value as output on your screen.



