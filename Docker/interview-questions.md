# Docker Interview Questions

**1. How to check logs from my docker container and how to filter only last 200
lines from container logs?**

- **Answer:** To check logs from a Docker container, you can use the
  `docker logs` command. To filter only the last 200 lines from the container
  logs, you can use the `-n` flag with the `docker logs` command as follows:

  ```bash
  docker logs --tail 200 container_name_or_id
  ```

**2. What will happen to container logs if you restart the container? Will it be
lost?**

- **Answer:** If you restart a Docker container, its logs will still be
  available. Docker preserves the logs of a container even after it restarts.
  Therefore, the logs won't be lost.

  If you stop and start a Docker container, the logs will still be available.
  Docker preserves the logs of a container even after it stops and starts again.
  Therefore, the logs won't be lost.

  However, if you delete a Docker container, its logs will be lost. Docker
  removes all resources associated with the container, including its logs, when
  it is deleted. Therefore, it's important to extract or archive the logs if
  they need to be preserved before deleting the container.

**3. You encounter a Docker image with size 2.7GB. Will this be a cause of
concern? If yes, how would you tackle this?**

- **Answer:** Yes, a Docker image size of 2.7GB could be a concern, especially
  in scenarios where network bandwidth and storage space are limited, or when
  deploying to resource-constrained environments like edge devices. To tackle
  this, you can employ several strategies such as optimizing Dockerfile
  instructions to minimize layer size, using multi-stage builds, removing
  unnecessary dependencies or files, utilizing Docker image layer caching
  effectively, and leveraging Docker image registries that support image
  compression and deduplication.

**4. What is the difference between docker image and docker layers?**

- **Answer:**A Docker image is a read-only template that contains a set of
  instructions for creating a container. It includes everything needed to run a
  container, such as the operating system, application code, dependencies, and
  runtime environment. Docker layers, on the other hand, are the intermediate
  filesystem snapshots created during the Docker image build process. Each
  Docker image is composed of multiple layers, with each layer representing a
  set of filesystem changes. Docker uses Union File System (UFS) to overlay
  these layers, providing a unified view of the filesystem to the container.

  **Images are built from layers:** Every instruction in your Dockerfile (like
  installing software) creates a layer. **Layers make images efficient:** If you
  have two images that share some base software, they can reuse the same layers,
  saving space. **Images are read-only:** They're the template. Containers are
  the actual "houses" you build and live in, based on the image.

**5. Explain CMD and ENTRYPOINT in Docker. Are these 2 same or different?**

- **Answer:** `CMD` and `ENTRYPOINT` are both instructions used in Dockerfiles
  to specify what command to run when a container starts. However, they serve
  different purposes and behave differently:

  - `CMD` is used to provide default arguments for the command that will be
    executed when the container starts. If the Dockerfile includes multiple
    `CMD` instructions, only the last one will take effect.

  - `ENTRYPOINT` is used to specify the executable to run when the container
    starts. It also allows you to pass arguments to the executable. If the
    Dockerfile includes multiple `ENTRYPOINT` instructions, only the last one
    will take effect. Additionally, you can override `ENTRYPOINT` at runtime by
    specifying a new command when running the container.
