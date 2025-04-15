## Important commands in Docker

We will quickly get acquainted with the common commands that you need to know to use Docker. These commands are as follows:

- **Command**: `docker run nginx`
  - **Details**: Creates and starts a container from the `nginx` image, running the Nginx web server. If the image isn’t present locally, it pulls it from a registry (e.g., Docker Hub).
- **Command**: `docker ps --all`
  - **Details**: Lists all containers on the system, including those that are stopped, not just the running ones. Without `--all`, it only shows running containers.
- **Command**: `docker stop container_id`
  - **Details**: Stops a running container specified by its `container_id`. Replace `container_id` with the actual ID of the container (e.g., obtained from `docker ps`).
- **Command**: `docker rm`
  - **Details**: Note: This command as listed is incomplete in the text (missing an argument). Typically, it should be `docker rm container_id` to remove a stopped container by its ID. Here, it’s assumed to indicate the general purpose of removing a container.
- **Command**: `docker pull nginx`
  - **Details**: Downloads the `nginx` image from a registry (e.g., Docker Hub) to the local system without running it.
- **Command**: `docker network ls`
  - **Details**: Lists all networks available in the Docker environment, showing network names, drivers, and scopes.
- **Command**: `docker logs container_id`
  - **Details**: Displays the logs of a container specified by its `container_id`. Replace `container_id` with the actual ID to see output or errors from the container.
- **Command**: `docker exec -it container_id`
  - **Details**: Executes a command (e.g., a shell) inside a running container specified by `container_id` in interactive (`-i`) and terminal (`-t`) mode. Replace `container_id` with the actual ID. Note: The specific command to execute isn’t provided in the text (e.g., typically `docker exec -it container_id bash`).
- **Command**: `docker-compose up`
  - **Details**: Starts all services defined in a `docker-compose.yml` file, creating and running containers, networks, and volumes as specified.
- **Command**: `docker-compose down`
  - **Details**: Stops and removes all containers, networks, and volumes created by `docker-compose up` based on the `docker-compose.yml` file.
