# CI/CD Pipeline Documentation

This repository uses GitHub Actions for continuous integration and continuous deployment.

## Workflow Overview

The CI/CD pipeline consists of the following stages:

1. **Linting**: Checks Python code quality using flake8
2. **Building and Pushing**: Builds the Docker image and pushes it to GitHub Container Registry (GHCR)

## Workflow Triggers

The workflow is triggered on:
- Push to the `main` branch
- Pull requests targeting the `main` branch
- Manual trigger using the "workflow_dispatch" event

## Jobs Details

### Lint

This job:
- Sets up Python 3.10
- Installs flake8 and project dependencies
- Runs flake8 to check for syntax errors and code quality issues

### Build and Push

This job:
- Sets up Docker Buildx for efficient image building
- Logs in to GitHub Container Registry (for non-PR events)
- Extracts metadata for Docker image tagging
- Builds the Docker image and pushes it to GHCR (for non-PR events)

## Image Tags

The Docker images are tagged with:
- Short SHA of the commit
- Branch name
- "latest" (only for the main branch)

## Required Secrets

The workflow uses `GITHUB_TOKEN` which is automatically provided by GitHub Actions.

## Usage

To use the Docker image from the GitHub Container Registry:

```bash
# Pull the image
docker pull ghcr.io/git-fossee/python-automation:latest

# Run the image
docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $PWD/<FILE_NAME>:/app/main.py ghcr.io/git-fossee/python-automation:latest main.py
```

## Package Visibility

By default, packages published to GHCR are private. To make them public, go to the package settings on GitHub after the first successful workflow run. 