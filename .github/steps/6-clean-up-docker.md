<!--
  <<< Author notes: Step 6 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

## Step 6: Clean up Docker container and image

Excellent! Now it's time to clean up your Docker environment! :broom:

Let's stop :stop_sign: the Docker container we have running and (optionally) remove the image. :wastebasket:

### :keyboard: Activity: Stop your container

1. Find your running container by typing `docker ps`.
1. Use the following command to stop your container:
   ```bash
   docker stop CONTAINER_ID
   ```
1. Replace `CONTAINER_ID` with your image's Container ID.
1. Press **Enter**.

### :keyboard: Activity: (optional) Remove your image

1. List Docker images on your system by typing `docker image ls`.
1. Use the following command to remove your Docker image:
   ```bash
   docker rmi IMAGE_ID
   ```
1. (optional) For the sake of verification, you can re-run `docker image ls` to confirm your Docker image has been removed.
1. _We can't automatically verify this step for you, so please continue on to the next step below!_
