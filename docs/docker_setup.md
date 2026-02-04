# Islandora Workbench Docker Setup

This guide provides instructions on how to build and use a Docker container for Islandora Workbench. The Docker image facilitates running Islandora Workbench with all necessary dependencies encapsulated, making it easier to perform Islandora migrations without worrying about environment configuration.

## Prerequisites

- **Docker**: Ensure Docker is installed on your system. You can install Docker from [here](https://docs.docker.com/get-docker/).

## Building the Docker Image

To build the Docker image, follow these steps:

1. **Clone the Repository**: 
   ```bash
   git clone https://github.com/mjordan/islandora_workbench.git
   cd islandora_workbench
   ```

2. **Build the Docker Image**:
   Use the following command to build the Docker image. This will create a user in the container that matches your host system's user ID (UID) and group ID (GID) to avoid permission issues when reading from or writing to mounted directories.
   ```bash
   docker build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) -t workbench-docker .
   ```

## Using the Docker Image

To run the Islandora Workbench container and execute a task, use the following command. The `-v` flags are used to pass directories from your host system to the Docker container so that Islandora Workbench can access the necessary files.

- The first `-v .:/workbench` flag mounts the current directory (where your Workbench configuration and data files are located) into the container's `/workbench` directory.
- The additional `-v` flags are for mounting other directories that your migration scripts may need to access. These could include directories containing files that Workbench will read from or write to during the migration process.

Example:

```bash
docker run -it --rm --network="host" \
    -v .:/workbench \
    -v /path/to/your/tmp:/tmp \
    -v /path/to/your/files:/mnt/data/local \
    --name update_existing_objects workbench-docker \
    bash -lc "./workbench --config /workbench/prod/update_islandora_objects.yml --check"
```

### Explanation of the Flags:

- `-v .:/workbench`: Mounts the current directory containing the Workbench configuration and data files to `/workbench` inside the container.
- `-v /path/to/your/tmp:/tmp`: Mounts the `/tmp` directory where Workbench may write temporary files (optional).
- `-v /path/to/your/files:/mnt/data/local`: Mounts a directory containing files required by your migration scripts (optional).
- `--network="host"`: Uses the host's network settings. This is particularly useful if your Workbench configuration needs to connect to services running on your local machine.
- `--rm`: Automatically removes the container when it exits.
- `--name update_existing_objects`: Names the container `update_existing_objects` for easy identification and holds no functional purpose.
- `bash -lc "./workbench --config /workbench/prod/update_islandora_objects.yml --check"`: Executes the Workbench with the specified configuration file. This example is checking the migration. Remove the `--check` and run it again if it passes.

## Troubleshooting

### Permission Issues

If you encounter permission errors, ensure the directories you are mounting have the correct permissions. The Docker container runs as a non-root user, matching your host system's UID and GID.

### Network Connection Errors or Other Common Errors

If Workbench fails to connect to your Islandora instance, verify the following:
- The Islandora server is running and accessible from your Docker container.
- The "host" configuration in your Workbench YAML file is correct.
- You have read/write permissions to the migration.log file.
- If it fails to build try disabling the buildkit to use the traditional build system, which might be necessary for compatibility reasons or specific build requirements.
   ```bash
      DOCKER_BUILDKIT=0 docker build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) -t workbench-docker .
   ```
For more details on using and configuring Islandora Workbench, refer to the official [Islandora Workbench documentation](https://github.com/mjordan/islandora_workbench_docs).
