# 🚀 Azure DevOps Pipeline for Python Test Automation with Poetry + Pytest

This document guides you through setting up a **CI pipeline** in **Azure DevOps** for a Python project using `pytest` for testing and `poetry` for dependency management. It follows DevOps best practices like YAML-based pipelines, test result publishing, and code coverage reporting.

---

## ✅ Prerequisites

Before starting, make sure you have:

- An Azure DevOps organization and project
- Code pushed to Azure Repos (or GitHub connected)
- A Python project using `poetry` and `pytest`
- A `pyproject.toml` with dependencies defined

---

## 🗂 Project Structure Example

```
my-python-project/
├── my_package/
│   └── __init__.py
├── tests/
│   └── test_example.py
├── pyproject.toml
└── README.md
```

---

## 🛠 Poetry Setup

Install and configure dependencies using Poetry:

### `pyproject.toml` example:

```toml
[tool.poetry]
name = "my-python-project"
version = "0.1.0"
description = "Python automation test project"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.11"

[tool.poetry.dev-dependencies]
pytest = "^8.0"
pytest-cov = "^5.0"
```

Install everything locally:

```bash
poetry install
```

---

## 🔧 Azure DevOps YAML Pipeline

Save this as `azure-pipelines.yml` in your project root.

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  pythonVersion: '3.11'

stages:
  - stage: BuildAndTest
    jobs:
      - job: PythonPoetryTestJob
        steps:
          - task: UsePythonVersion@0
            inputs:
              versionSpec: '$(pythonVersion)'
              addToPath: true

          - script: |
              python -m pip install --upgrade pip
              pip install poetry
            displayName: 'Install Poetry'

          - script: |
              poetry install
            displayName: 'Install Dependencies with Poetry'

          - script: |
              poetry run pytest --junitxml=test-results/results.xml --cov=your_package_name --cov-report=xml
            displayName: 'Run Pytest with Coverage'

          - task: PublishTestResults@2
            inputs:
              testResultsFormat: 'JUnit'
              testResultsFiles: '**/results.xml'
              failTaskOnFailedTests: true

          - task: PublishCodeCoverageResults@1
            inputs:
              codeCoverageTool: 'Cobertura'
              summaryFileLocation: 'coverage.xml'
              reportDirectory: ''
```

> 🔁 Replace `your_package_name` with the actual name of your Python package/module.

---

## 🧪 Best Practices

- ✅ Commit `pyproject.toml` and lock files to source control.
- ✅ Use `poetry` for isolated, reproducible environments.
- ✅ Automate tests on pull requests with Azure Repos branch policies.
- ✅ Publish test results and coverage metrics.
- ✅ Separate test and production stages in multi-stage pipelines.
- ✅ Secure credentials using variable groups and Azure Key Vault.

---

## 🌐 Azure Pipelines Foundation Guide

This section expands on the general Azure Pipeline knowledge every team member should know.

---

## 📘 Table of Contents

1. [What Is Azure Pipelines?](#what-is-azure-pipelines)
2. [Core Concepts](#core-concepts)
3. [Pipeline Types](#pipeline-types)
4. [YAML Pipeline Basics](#yaml-pipeline-basics)
5. [Stages of a Typical Pipeline](#stages-of-a-typical-pipeline)
6. [Pipeline Setup Step-by-Step](#pipeline-setup-step-by-step)
7. [Best Practices](#best-practices)
8. [Environments & Approvals](#environments--approvals)
9. [Security & Secrets Management](#security--secrets-management)
10. [Extensions & Integrations](#extensions--integrations)
11. [Resources](#resources)

---

## 📌 What Is Azure Pipelines?

Azure Pipelines is a service within Azure DevOps that automates:

- **Builds**: Compiling source code, running tests, and generating artifacts
- **Releases**: Deploying code to staging or production environments
- **CI/CD**: Continuous Integration and Continuous Deployment

Supports:
- All major platforms: Windows, Linux, macOS
- Multiple languages: Python, .NET, JavaScript, Java, Go, PHP, etc.
- Any repo: Azure Repos, GitHub, Bitbucket, GitLab

---

## 📚 Core Concepts

| Concept        | Description |
|----------------|-------------|
| **Pipeline**   | Defines automation workflow (build, test, deploy). |
| **Stage**      | Major group of jobs (e.g., Build, Test, Deploy). |
| **Job**        | A series of steps executed on an agent. |
| **Step**       | A task or script executed in a job. |
| **Agent**      | VM or container that runs the pipeline. |
| **Artifact**   | Output of a pipeline (e.g., binaries, zips). |
| **Environment**| Logical group of resources with approval gates. |

---

## 🛠 Pipeline Types

- **Classic UI Pipelines**: Created via drag-and-drop; deprecated in favor of YAML.
- **YAML Pipelines** (Recommended): Code-based, version-controlled, portable.

---

## 🧱 YAML Pipeline Basics

Minimal YAML file example:

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - script: echo "Hello, world!"
```

Pipeline files are stored as `azure-pipelines.yml` in the root of your repo.

---

## 🧪 Stages of a Typical Pipeline

1. **Build Stage**
   - Install dependencies
   - Compile code
   - Run unit tests
   - Generate artifacts

2. **Test Stage**
   - Run integration, E2E, or smoke tests
   - Report code coverage
   - Publish test results

3. **Release Stage**
   - Deploy to `dev`, `staging`, or `prod`
   - Trigger approvals
   - Run smoke tests post-deploy

---

## 🔧 Pipeline Setup Step-by-Step

### 1. Create Azure DevOps Project

1. Navigate to [https://dev.azure.com](https://dev.azure.com)
2. Click **"New Project"**, set name/visibility
3. Import or push your source code

### 2. Create `azure-pipelines.yml` in your repo

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - script: echo "Build and test"
```

### 3. Commit and Push — Pipeline is auto-triggered

---

## 🚦 Environments & Approvals

Define environments for deployment targets:

```yaml
jobs:
  - deployment: DeployStaging
    environment: 'staging'
    strategy:
      runOnce:
        deploy:
          steps:
            - script: echo "Deploying to staging"
```

Enable **pre-deployment approvals** in Azure UI to add team validation.

---

## 🔐 Security & Secrets Management

- Use **variable groups** to share values across pipelines
- Link variable groups to **Azure Key Vault** for secret injection
- Secure **Service Connections** with RBAC
- Use **scoped permissions** to restrict pipeline access

---

## 🔌 Extensions & Integrations

Azure Pipelines integrates with:

- **GitHub, Bitbucket, GitLab**
- **Slack, Microsoft Teams**
- **Terraform, Pulumi, Bicep**
- **SonarCloud** for static code analysis
- **Snyk, Trivy, Defender** for security scanning
- **DockerHub, Kubernetes, App Service**

Marketplace: [https://marketplace.visualstudio.com/azuredevops](https://marketplace.visualstudio.com/azuredevops)

---

## 📚 Resources

- [Azure DevOps Documentation](https://learn.microsoft.com/en-us/azure/devops/)
- [YAML Schema Reference](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema)
- [Azure DevOps Labs](https://azuredevopslabs.com/)
- [Secure DevOps Kit](https://github.com/azsk/DevOpsKit)
- [Poetry](https://python-poetry.org/docs/)
- [Pytest](https://docs.pytest.org/)

---

## ✅ Summary Checklist

| Feature                          | Configured? |
|----------------------------------|-------------|
| YAML pipeline in source control | ✅          |
| CI triggered on `main` branch    | ✅          |
| Automated tests with results     | ✅          |
| Code coverage reports            | ✅          |
| Build artifacts published        | ✅          |
| Multi-environment deployments    | ✅          |
| Secrets from Key Vault           | ✅          |
| Approvals for production deploys| ✅          |

---

## 📄 License

This guide is MIT-licensed. You are free to use and modify it for your team's internal documentation or projects.