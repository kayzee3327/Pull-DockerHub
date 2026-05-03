# pull_dockerhub

Run a workflow to pull from dockerhub and push to other container registry.

This can be helpful when connection to dockerhub is restricted.

# Use secret to hardcode username, registry and password

It is convenient to hardcode some credential information in workflows. Replace actual information with github repo secrets for public repos.

# After pulling from target registry to local machine

### Rename the image

Rename the image first since third party may give a very long tag, such as `example-registry.com/tensorflow:2.19.0-gpu-jupyter`.

1. list all images, and find `IMAGE ID` or `REPOSITORY:TAG` of the target image
   ```bash
   docker images
   ```
   Example Output:
   ```
   REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
   old-image    latest    abcdef123456   2 weeks ago    1.2GB
   ```
2. `docker tag` to copy the image with a new name
   ```bash
   docker tag <old image name>:<old tag> <new image name>:<new tag>
   ```
   or
   ```bash
   docker tag <IMAGE ID> <new image name>:<new tag>
   ```
3. run `docker images` again, you will see both old and new image
   ```
   REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
   old-image    latest    abcdef123456   2 weeks ago    1.2GB
   new-image    latest    abcdef123456   2 weeks ago    1.2GB
   ```
4. (optional) remove the old image
   ```bash
   docker rmi old-image:latest
   ```
