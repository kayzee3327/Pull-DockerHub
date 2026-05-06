# Pull-DockerHub

This repository contains several automated workflows designed to pull container images from Docker Hub and push them to an alternate container registry. 

These pipelines utilize Github Actions to run push and pull of container images on a temporary Github server, which is ideal for **bypassing connectivity restrictions associated with Docker Hub**. 

## Use Repo Action Secret for Authentication Configuration

To adhere to security best practices, never hardcode credentials directly within the workflow definitions. Instead, securely inject the necessary authentication parameters utilizing GitHub Repository Secrets:

- Target Registry Endpoint

- Registry Username

- Registry Access Token / Password

Secrets can be managed in `Settings -> Secrets and Variables -> Actions` by the repo owner.

## Local Image Management: Retagging

Alternate registries often append extensive prefixes to image names (e.g., registry.example.com/namespace/tensorflow:2.19.0-gpu-jupyter). Upon pulling the mirrored image to a local environment, you may retag it to establish a standardized naming convention.

1. **Locate the Image:** List local images to identify the target `IMAGE ID` or current `REPOSITORY:TAG`.
   ```bash
   docker images
   ```
   Example Output:
   ```
   REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
   old-image    latest    abcdef123456   2 weeks ago    1.2GB
   ```
2. **Retag the Image:** Create a new reference pointing to the existing image.
   ```bash
   docker tag <old image name>:<old tag> <new image name>:<new tag>
   ```
   Alternatively, using the Image ID:
   ```bash
   docker tag <IMAGE ID> <new image name>:<new tag>
   ```
5. Run `docker images` again, and you will see both old and new image
   ```
   REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
   old-image    latest    abcdef123456   2 weeks ago    1.2GB
   new-image    latest    abcdef123456   2 weeks ago    1.2GB
   ```
6. **Cleanup (Optional):** Verify the successful creation of the new tag via docker images, then safely remove the original, verbose reference to optimize local storage readability.
   ```bash
   docker rmi old-image:latest
   ```
