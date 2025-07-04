# CICD demo

## Project Overview

This project demonstrates a minimal Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub Actions and GitHub Pages to automatically deploy a static HTML site.

---

## Live Demo

You can view the deployed project here:
[https://gastondvoskin.github.io/cicd-demo/](https://gastondvoskin.github.io/cicd-demo/)

---

## Quick Start: Clone and Deploy

Follow these steps to clone this repository and deploy your own static site using GitHub Actions and GitHub Pages.

### 1. Clone the Repository

```sh
git clone https://github.com/gastondvoskin/cicd-demo.git
cd cicd-demo
```

### 2. (Optional) Edit the HTML File

You can edit `index.html` to customize your page:

### 3. Push Your Changes to GitHub

```sh
git add .
git commit -m "Update index.html or other files"
git push origin main
```

### 4. GitHub Actions Workflow

The workflow file is already set up at `.github/workflows/deploy-github-page.yml`. On every push to the `main` branch, your site will be automatically deployed to GitHub Pages.

#### Workflow configuration:

```yaml
name: Deploy GitHub Page

on:
  push:
    branches:
      - main

permissions:
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "."
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v4
```

### 5. Enable GitHub Pages

1. Go to your repository on GitHub.
2. Navigate to **Settings > Pages**.
3. Under **Build and deployment**, select **GitHub Actions** as the source.

### 6. Access Your Deployed Site

After the workflow completes, your site will be available at:

```
https://<your-username>.github.io/<your-repo>/
```

For this repository:
[https://gastondvoskin.github.io/cicd-demo/](https://gastondvoskin.github.io/cicd-demo/)

---

## (Optional) Install GitHub Actions Extension for VS Code

- Open VS Code.
- Go to the Extensions panel (Ctrl+Shift+X or Cmd+Shift+X).
- Search for "GitHub Actions" and install the official extension.
- This extension allows you to:
  - View workflow runs and logs directly in VS Code.
  - Trigger workflows manually.
  - Get autocompletion and validation for workflow YAML files.

---

## Adding Another GitHub Actions Workflow

You can add as many workflows as you need. To add a new workflow:

1. Create a new YAML file in the `.github/workflows/` directory, for example:
   ```sh
   touch .github/workflows/another-workflow.yml
   ```
2. Add your workflow configuration to this file. Each workflow runs independently and can be triggered by different events (push, pull request, schedule, etc).

---

## What is a Workflow YAML File?

A workflow YAML file defines automated processes for your repository. It consists of several key parts:

- **name:** The name of the workflow (for display in GitHub).
- **on:** The event(s) that trigger the workflow (e.g., push, pull_request).
- **jobs:** One or more jobs to run. Each job runs on a virtual machine.
- **steps:** The sequence of commands or actions to execute in each job.
- **uses:** Calls a reusable action from the GitHub Marketplace or a public repo.
- **run:** Runs a shell command directly.

Example structure:

```yaml
name: Example Workflow
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Example step
        run: echo "Hello, World!"
```

---

## Finding Workflow Templates on GitHub

- Visit the [GitHub Actions Marketplace](https://github.com/marketplace?type=actions) to find reusable actions and workflow templates.
- You can also search for "workflow templates" or "actions" in the GitHub Marketplace or directly in the [Actions tab](https://github.com/features/actions) of your repository.
- When creating a new workflow in GitHub, you can choose from a variety of starter templates for common use cases (Node.js, Python, deployment, etc).

---
