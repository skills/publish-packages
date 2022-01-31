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



