# Python Automation

[![Python Application CI/CD](https://github.com/git-fossee/python-automation/actions/workflows/ci-cd.yml/badge.svg)](https://github.com/git-fossee/python-automation/actions/workflows/ci-cd.yml)

Pre requisite:

Set docker image name
```bash
export IMAGE_NAME=<SET YOUR DOCKER IMAGE NAME>
```

Set docker image tag
```bash
export IMAGE_TAG=<SET YOUR DOCKER IMAGE TAG>
```

To build the docker image
```bash
docker build -t $IMAGE_NAME:$IMAGE_TAG .
```

Allow xserver for docker on local connection
```bash
sudo xhost +local:docker
```

To run:

```bash
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $PWD/<FILE_NAME>:/app/main.py $IMAGE_NAME:$IMAGE_TAG main.py
```

## CI/CD Pipeline

This repository includes a GitHub Actions CI/CD pipeline that:
1. Lints the Python code
2. Builds and pushes the Docker image to GitHub Container Registry

For more details, see [CI/CD Documentation](docs/ci-cd.md).

### Using the published image

You can directly use the image from GitHub Container Registry:

```bash
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $PWD/<FILE_NAME>:/app/main.py ghcr.io/git-fossee/python-automation:latest main.py
```
