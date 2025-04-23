# Python Automation

[![Python Application CI/CD](https://github.com/git-fossee/python-automation/actions/workflows/ci-cd.yml/badge.svg)](https://github.com/git-fossee/python-automation/actions/workflows/ci-cd.yml)

A Python automation toolkit with Docker containerization, providing a consistent environment for running Python scripts that require GUI capabilities.

## Table of Contents
- [Features](#features)
- [Local Development](#local-development)
- [CI/CD Pipeline](#cicd-pipeline)
- [Contributing](#contributing)

## Features
- Containerized Python environment with GUI support
- Pre-installed dependencies for automation tasks
- Integration with GitHub Actions for CI/CD
- Automated Docker image builds

## Local Development

### Prerequisites
- Docker installed on your machine
- X server for GUI applications

### Building Locally

Set docker image name:
```bash
export IMAGE_NAME=<SET YOUR DOCKER IMAGE NAME>
```

Set docker image tag:
```bash
export IMAGE_TAG=<SET YOUR DOCKER IMAGE TAG>
```

Build the docker image:
```bash
docker build -t $IMAGE_NAME:$IMAGE_TAG .
```

Allow xserver for docker on local connection:
```bash
sudo xhost +local:docker
```

Run your Python script:
```bash
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $PWD/<FILE_NAME>:/app/main.py $IMAGE_NAME:$IMAGE_TAG main.py
```

## CI/CD Pipeline

This repository includes a GitHub Actions CI/CD pipeline that:
1. Lints the Python code using flake8
2. Builds the Docker image
3. Pushes the image to GitHub Container Registry (GHCR)

### CI/CD Workflow Triggers
- Push to the `main` branch
- Pull requests targeting the `main` branch
- Manual trigger from the Actions tab

### Using the Published Image

You can directly use the image from GitHub Container Registry:

```bash
docker pull ghcr.io/git-fossee/python-automation:latest
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $PWD/<FILE_NAME>:/app/main.py ghcr.io/git-fossee/python-automation:latest main.py
```

For more details, see [CI/CD Documentation](docs/ci-cd.md).

## Contributing

### Testing Locally
Before submitting a pull request, please ensure:
1. Your code passes the linting checks:
   ```bash
   pip install flake8
   flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
   ```

2. The Docker image builds successfully:
   ```bash
   docker build -t test-image .
   ```

3. Your script runs correctly in the container

### Pull Request Process
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

The CI/CD pipeline will automatically run on your pull request to validate changes.
