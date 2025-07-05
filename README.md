# CICD Demo

## Project Overview

This repository demonstrates a minimal Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub Actions, with three independent subprojects:

- **minimal-test/**: A Node.js project with automated tests.
- **deploy-gh-page/**: A static site deployed to GitHub Pages.
- **hello-world/**: A simple directory for workflow demonstration.

Each subproject has its own workflow and can be tested independently.

---

## Project Structure

```
cicd-demo/
├── minimal-test/         # Node.js project with tests
├── deploy-gh-page/       # Static site for GitHub Pages
├── hello-world/          # Simple workflow demo
└── .github/workflows/    # All workflow YAML files
```

---

## Basic Structure of a GitHub Actions Workflow YAML

A workflow YAML file defines automated processes for your repository. The most common fields are:

```yaml
name: Example Workflow # (Optional) The name of the workflow as shown in the Actions tab

on: # The event(s) that trigger the workflow
  push: # Run on push events
    branches:
      - main # Only run on pushes to the 'main' branch
  pull_request: # Run on pull request events

jobs: # A workflow is made up of one or more jobs
  build: # The job ID (can be any name)
    runs-on: ubuntu-latest # The type of virtual machine to use
    steps: # A job contains a sequence of steps
      - name: Checkout code # Step name (for display)
        uses: actions/checkout@v4 # Use a prebuilt action from the marketplace
      - name: Run tests
        run: npm test # Run a shell command
```

**Example minimal workflow:**

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello, World!"
```

---

## How to Test Each Subproject

### 1. **minimal-test/**

- Contains a Node.js project with Jest tests.
- Workflow: `.github/workflows/minimal-test.yml`
- **To test locally:**
  ```sh
  cd minimal-test
  npm install
  npm test
  ```
- **To test via CI:**
  - Push changes to `minimal-test/` or to the workflow file.
  - The workflow will run automatically on GitHub Actions for `main` and `develop` branches.

### 2. **deploy-gh-page/**

- Contains static HTML files for deployment.
- Workflow: `.github/workflows/deploy-github-page.yml`
- **To test locally:**
  - Open `deploy-gh-page/index.html` in your browser.
- **To deploy via CI:**
  - Push changes to `deploy-gh-page/` or to the workflow file.
  - The workflow will build and deploy the site to GitHub Pages.

### 3. **hello-world/**

- Simple directory for workflow demonstration.
- Workflow: `.github/workflows/hello-world.yml`
- **To test via CI:**
  - Push changes to `hello-world/` or to the workflow file.
  - The workflow will run a simple echo command on GitHub Actions.

---

## Setting Up GitHub Rulesets (Branch Protection)

To ensure code quality and prevent broken code from being merged, follow these steps to configure rulesets in your repository:

### **Step-by-Step Guide:**

1. **Go to your repository on GitHub.**
2. **Navigate to:** `Settings` → `Code and automation` → `Rules` → `Rulesets`.
3. **Click `New ruleset` or edit an existing one.**
4. **Set the Ruleset Name:**
   - Example: `branch-protection` or `main-branch-protection`.
5. **Target Branches:**
   - Choose `main` (recommended) or any branches you want to protect.
6. **Enforcement Status:**
   - Set to `Active` to enforce the rules.
7. **Require status checks to pass:**
   - Enable this option.
   - Click `Add checks` and select the relevant status checks (e.g., `minimal-test`, `deploy-github-page`, etc.).
8. **(Recommended) Require a pull request before merging:**
   - Ensures all changes go through a PR and CI before merging.
9. **Block force pushes and restrict deletions:**
   - Enable for extra safety.
10. **Save the ruleset.**

**Result:** Now, any push or merge to the protected branch will require all status checks to pass before being accepted.

---

## Example: How to Intentionally Fail a Status Check

Suppose you want to verify that your CI/CD pipeline blocks broken code. You can intentionally break a test in `minimal-test/`:

**Edit `minimal-test/index.js`:**

```js
// Original sum function
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

**Change it to:**

```js
// Intentionally broken sum function
function sum(a, b) {
  return a - b; // This will cause the test to fail
}
module.exports = sum;
```

**Commit and push the change:**

```sh
git add minimal-test/index.js
git commit -m "Update sum function to intentionally fail the check"
git push origin <your-branch>
```

**Expected result:**

- The GitHub Actions workflow for `minimal-test` will fail.
- If you have branch protection rules, you will not be able to merge this change until the test passes.

---

## Troubleshooting

- **Cannot push to protected branch:**
  - Make sure you are pushing to a branch that is not protected, or create a Pull Request for merging.
- **Status check is expected but not running:**
  - Ensure your workflow YAML files include all relevant branches in the `on.push.branches` section.
- **Workflow not appearing in status checks:**
  - The status check name must match the job name in your workflow YAML.

---

## Important Clarification: Branch Protection, Status Checks, and Workflow Triggers

If your `main` branch is protected by a ruleset that requires a status check, and your workflow is only triggered by `on: push` to `main`, **you will not be able to push directly to `main`** (as the check cannot run before the push is accepted).

Additionally, if you want to create a pull request from `develop` to `main` and require status checks to pass, you must include `develop` in the workflow trigger (e.g., `on: push` branches: `main` and `develop`).

Otherwise, the status check will remain pending on the PR, because the workflow will not run for changes in `develop`.

---
