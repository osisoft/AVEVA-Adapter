---
uid: InstallUsingDocker
---

# Installation using Docker

Docker is a set of tools that you can use on Linux to manage application deployments.

**Note:** If you want to use Docker, you must be familiar with the underlying technology and have determined that it is appropriate for your planned use of the adapter. Docker is not required to use the adapter.

This topic provides examples of how to create a Docker container with the adapter.

## Create a startup script

To create a startup script for the adapter, follow the instructions below.

1. Use a text editor to create a script similar to one of the following examples:

    **Note:** The script varies slightly by processor.

    **ARM32**

    [!include[startup script: ARM32](../_includes/block/startup-scripts/arm32.md)]

    **ARM64**

    [!include[startup script: ARM32](../_includes/block/startup-scripts/arm32.md)]

    **AMD64**
            
    [!include[startup script: ARM32](../_includes/block/startup-scripts/amd64.md)]

2. Name the script `eventhubsdockerstart.sh` and save it to the directory where you plan to create the container.

## Create a Docker container

To create a Docker container that runs the adapter, follow the instructions below.

1. Create the following `Dockerfile` in the directory where you want to create and run the container.

    **Note:** `Dockerfile` is the required name of the file. Use the variation according to your operating system:

    **ARM32**

    [!include[docker files: ARM32](../_includes/block/dockerfiles/arm32.md)]

    **ARM64**

    [!include[docker files: ARM64](../_includes/block/dockerfiles/arm64.md)]
    
	**AMD64 (x64)**

    [!include[docker files: AMD64](../_includes/block/dockerfiles/amd64.md.md)]

2. Copy the [!include[installer](../_includes/inline/installer-name.md)]-<platform>_.tar.gz file to the same directory as the `Dockerfile`.

3. Copy the <code>[!include[startup-script](../_includes/inline/startup-script.md)]</code> script to the same directory as the `Dockerfile`.

4. Run the following command line in the same directory (`sudo` may be necessary):

	<!-- Customize for adapter, EG: eventhubsadapter -->

    ```bash
    docker build -t <startup-script> .
    ```

## Docker container startup

The following procedures contain instructions on how to run the adapter inside a Docker container with different options enabled.

### Run the Docker container with REST access enabled

To run the adapter inside a Docker container with access to its REST API from the local host, complete the following steps:

1. Use the docker container image <code>[!include[docker-image](../_includes/inline/docker-image.md)]</code> created previously.

2. Type the following in the command line (`sudo` may be necessary):

	<!-- Customize for adapter, EG: eventhubsadapter-->

    ```bash
    docker run -d --network host <docker-image>
    ```

Port `5590` is accessible from the host and you can make REST calls to the adapter from applications on the local host computer. In this example, all data stored by the adapter is stored in the container itself. When you delete the container, the stored data is also deleted.

### Run the Docker container with persistent storage

To run the adapter inside a Docker container while using the host for persistent storage, complete the following steps. This procedure also enables access to the adapter REST API from the local host.

1. Use the docker container image <code>[!include[docker-image](../_includes/inline/docker-image.md)]</code> created previously.

2. Type the following in the command line (`sudo` may be necessary):

	<!-- Customize for adapter, EG: eventhubsadapter-->

    ```bash
    docker run -d --network host -v /<adapter>:/usr/share/OSIsoft/ <docker-image>
    ```

Port `5590` is accessible from the host and you can make REST calls to the adapter from applications on the local host computer. In this example, all data that is written to the container is instead written to the host directory and the host directory is a directory on the local machine, <!-- customize -->`/<adapter>`. You can specify any directory.

### Change port number

To use a different port other than `5590`, you can specify a `portnum` variable on the `docker run` command line. For example, to start the adapter using port `6000` instead of `5590`, use the following command:

<!-- Customize for adapter, EG: eventhubsadapter-->

```bash
docker run -d -e portnum=6000 --network host <docker-image>
```

This command accesses the REST API with port `6000` instead of port `5590`. The following `curl` command returns the configuration for the container.

```bash
curl http://localhost:6000/api/v1/configuration
```

### Remove REST access

If you remove the `--network host` option from the docker run command, REST access is not possible from outside the container. This may be of value where you want to host an application in the same container as the adapter but do not want to have external REST access enabled.
