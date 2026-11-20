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

## 🏗️ CI/CD Architecture

```text
             Developer
                 |
                 | git push
                 v
          GitHub Repository
                 |
                 v
           GitHub Actions
                 |
          +------+------+
          |             |
          v             v
      Node.js 20    Node.js 22
       npm test      npm test
          |             |
          +------+------+
                 |
           Tests Passed?
                 |
                 v
          Build Docker Image
                 |
                 v
          Authenticate to GHCR
                 |
                 v
          Push Docker Image
                 |
                 v
        GitHub Container Registry
                 |
                 v
       ghcr.io/pittaharika/actions-lab
```

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

| Technology | Purpose |
|---|---|
| Node.js | Application runtime |
| Jest | Unit testing |
| GitHub Actions | CI/CD automation |
| Docker | Containerization |
| GitHub Container Registry | Docker image storage |
| Git | Version control |
| YAML | Workflow configuration |

## 📋 Prerequisites

Install the following tools:

- Git
- Node.js 20 or newer
- npm
- Docker Desktop
- GitHub account

## 1. Create the Node.js Application

Initialize the Node.js project:

```bash
npm init -y
```

Install Jest:

```bash
npm install --save-dev jest
```

Configure the test script:

```bash
npm pkg set scripts.test="jest"
```

### sum.js

```javascript
function sum(a, b) {
  return a + b;
}

module.exports = sum;
```

### sum.test.js

```javascript
const sum = require('./sum');

test('adds 2 + 3 to equal 5', () => {
  expect(sum(2, 3)).toBe(5);
});
```

Run the tests:

```bash
npm test
```

Expected output:

```text
PASS ./sum.test.js

Tests: 1 passed, 1 total
```

## 2. Create the Dockerfile

Create a file named `Dockerfile`:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

CMD ["node", "-e", "console.log(require('./sum')(2,3))"]
```

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

```yaml
name: CI-CD Pipeline

on:
  push:
  pull_request:

permissions:
  contents: read
  packages: write

jobs:
  test:
    name: Test Node.js ${{ matrix.node-version }}
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [20, 22]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

  build-and-push:
    name: Build and Push Docker Image
    needs: test
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'

    runs-on: ubuntu-latest

