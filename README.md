# vscode_docker_playground

In this repository we will set up a reproducible and isolated environment for programming using Docker and VSCode. If you need a thorough dive into Docker, I recommend the [Docker documentation page](https://docs.docker.com/). For more details about VSCode, have a look at the [VSCode documentation page](https://code.visualstudio.com/docs).

## Building a Docker Image
Steps to build the Docker Images:
```
# For the cpu docker image
docker build -f docker/Dockerfile.cpu --build-arg UBUNTU_VERSION=18.04 --build-arg PYTHON_VERSION=3.7 -t docker_playground_cpu .

# For the gpu docker image
docker build -f docker/Dockerfile.gpu --build-arg CUDA_VERSION=11.2.1 --build-arg UBUNTU_VERSION=18.04 --build-arg PYTHON_VERSION=3.7 -t docker_playground_gpu .
```

## Running a Docker Container
Once you have built a Docker Image, you can run a Docker Container, that uses the Docker Image to create your isolated environment:
```
# Simple cpu image example
docker run -it \
    --mount type=bind,source=$HOME/code,target=/code \
    --mount type=bind,source=$HOME/datasets,target=/datasets \
    docker_playground_cpu /bin/zsh

# Simple gpu image example. Note that nvidia-docker needs to be installed!
docker run -it \
    --mount type=bind,source=$HOME/code,target=/code \
    --mount type=bind,source=$HOME/datasets,target=/datasets \
    --gpus all \
    docker_playground_gpu /bin/zsh
```

## Setting Up your development environment using Docker and VSCode
The `.devcontainer` folder specifies the arguments needed for VSCode to launch an isolated environment. In particular, the `.devcontainer/devcontainer.json` file specifies build arguments for building the Docker Image, the path to the Dockerfile recipe, the run arguments when running a container, and many more. For details you can visit the [devcontainer reference page](https://code.visualstudio.com/docs/remote/devcontainerjson-reference).

The steps to run the container in VSCode are the following:
1. Open the VSCode app.
2. Install the Remote - Containers VSCode extension (see details [here](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers))
3. Open the folder containing the .devcontainer folder (`File->Open Folder`). In our case this is `${HOME}/code/vscode_docker_playground`.
4. VSCode should prompt you that the folder contains a devcontainer file and you could run your code inside a Docker Container. Select `Build Image`. In case the prompt does not appear, go to `View->Command Palette` and type `Remote-Containers: Open Folder in Container`, pressing Enter to the command. Select the folder that contains the .devcontainer file, and the docker container will now be created for you!
