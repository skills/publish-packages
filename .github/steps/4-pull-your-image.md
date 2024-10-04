<!--
  <<< Author notes: Step 4 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 4: Pull your image

_Now things are running! :sparkles:_

Whoa, now things are running! This may take a few minutes. This might take a tiny amount of time, so grab your popcorn :popcorn: and wait for the build to finish before moving on.

:cook: While we wait for the build to finish, let's take care of a few prerequisites.

For ease of use and cross-platform compatibility (Windows, Mac, and Linux), we'll focus on Docker Desktop. Not to be confused, Docker Engine is the foundation for running containers while **[Docker Desktop](https://www.docker.com/blog/how-to-check-docker-version/)** bundles Docker Engine, a GUI, and a Virtual Machine in a _single installation_.

1. Install [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/#install-docker-desktop-on-windows).
   * If you're using Mac or Linux, locate the correct install steps at the previous link via the lefthand tree menu.
1. Open Docker Desktop and [briefly explore](https://docs.docker.com/desktop/use-desktop/).
1. For running `docker` commands, access the command-line terminal either via Bash, Git Bash, Windows Command Prompt or PowerShell.

:inbox_tray: To pull the Docker image, we need to log into Docker first.

Before we can use this Docker image, you will need to generate a [personal access token (classic)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) that contains the following permissions:

**Scopes for Personal Access Token (classic)** :coin:
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

1. Copy the `pull` command from the package instructions.
   - :fire: _Tip: To reach this page, click the **Code** tab at the top of your repository. Then, find the navigation bar below the repository description, and click the **Packages** heading link_
     ![screenshot of the pull command on the GitHub package page](https://user-images.githubusercontent.com/3250463/219254981-9ff949fa-4d01-46e3-9e3d-b8ce3710c2a9.png)
   - Or alternatively, in the following URL replace `YOURNAME`, `REPONAME`, and browse to `https://github.com/users/YOURNAME/packages?repo_name=REPONAME` and click on the package name
1. Replace `YOURNAME` with your GitHub username.
1. Replace `TAG` with the image tag.
1. Paste the `pull` command into your terminal. It should look something like this:
   - `docker pull ghcr.io/YOURNAME/publish-packages/game:TAG`
1. Press **Enter**.
1. You should see output indicating that the pull was successful, like `Status: Downloaded newer image for ghcr.io/YOURNAME/publish-packages/game:TAG`.
   ![screenshot of successful Docker image output](https://user-images.githubusercontent.com/3250463/219255178-3c943a6f-6c15-4f59-9002-228249b1c469.png)
1. _We can't automatically verify this step for you, so please continue on to the next step below!_
