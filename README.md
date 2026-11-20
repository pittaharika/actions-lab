# GitHub Actions CI/CD Pipeline with Docker

## Project Overview

This project demonstrates a CI/CD pipeline using GitHub Actions, Node.js, Docker, and GitHub Container Registry (GHCR).

## Technologies Used

- Git and GitHub
- GitHub Actions
- Node.js
- npm
- Docker
- GitHub Container Registry (GHCR)

## CI/CD Workflow

1. Developer pushes code to the main branch.
2. GitHub Actions automatically starts the workflow.
3. Automated tests run using Node.js 20 and 22.
4. Docker image is built.
5. Docker image is pushed to GitHub Container Registry.

## Key Features

- Automated testing
- Multi-version Node.js testing
- Docker image building
- Automated image publishing to GHCR
- GitHub Actions workflow automation

## Docker Image

`ghcr.io/pittaharika/actions-lab:latest`

## How to Run

```bash
docker pull ghcr.io/pittaharika/actions-lab:latest
docker run ghcr.io/pittaharika/actions-lab:latest
```

## Skills Demonstrated

CI/CD, GitHub Actions, Docker, Node.js, Git, and container registry management.
