# actions-lab

# CI/CD with GitHub Actions

A complete CI/CD pipeline using **GitHub Actions, Node.js, Docker, and GitHub Container Registry (GHCR)**.

This project demonstrates automated testing across multiple Node.js versions, Docker image building, caching, and publishing container images to GHCR.

## 🚀 Project Overview

The pipeline automatically:

- Runs unit tests on every push and pull request.
- Tests the application using Node.js 20 and Node.js 22 through a matrix strategy.
- Caches npm dependencies to speed up builds.
- Builds a Docker image after successful tests.
- Publishes the Docker image to GitHub Container Registry (GHCR) on pushes to the `main` branch.
- Uses GitHub's built-in `GITHUB_TOKEN` for secure authentication.


## 📁 Project Structure

```text
actions-lab/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── Dockerfile
├── .dockerignore
├── package.json
├── package-lock.json
├── sum.js
├── sum.test.js
└── README.md
```

## 🛠️ Technologies Used

 Technology         Purpose 

 Node.js            Application runtime 
 Jest               Unit testing 
 GitHub Actions     CI/CD automation 
 Docker             Containerization 
 GHCR               image storage 
 Git                Version control 
 YAML               Workflow configuration 

## 📋 Prerequisites

Install the following tools:

- Git
- Node.js 20 or newer
- npm
- Docker Desktop
- GitHub account

## 1. Create the Node.js Application


npm test


Expected output:

```text
PASS ./sum.test.js

Tests: 1 passed, 1 total
```

## 2. Create the Dockerfile

Create a file named `Dockerfile`:

### .dockerignore

```text
node_modules
.git
.github
npm-debug.log
```

Build the Docker image locally:

```bash
docker build -t actions-lab .
```

Run the container:

```bash
docker run --rm actions-lab
```

Expected output:

```text
5
```

## 3. Configure GitHub Actions

Create the workflow directory:

```bash
mkdir -p .github/workflows
```

Create `.github/workflows/ci.yml`:
