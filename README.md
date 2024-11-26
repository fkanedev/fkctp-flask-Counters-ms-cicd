![Python](https://img.shields.io/badge/Python-3.9-blue.svg)
![Built with Flask](https://img.shields.io/badge/Built%20with-Flask-b5f05d.svg)
[![CI workflow](https://github.com/fkanedev/fkctp-flask-Counters-ms-cicd/actions/workflows/workflow.yml/badge.svg)](https://github.com/fkanedev/fkctp-flask-Counters-ms-cicd/actions/workflows/workflow.yml)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

# Counter Management Microservice Project

This final project is an industry-based case study involving the development of a microservice for managing and tracking counters. I undertook this project as part of my training in the [IBM DevOps and Software Engineering Professional Certificate](https://www.coursera.org/professional-certificates/devops-and-software-engineering). It highlights the practical application of Continuous Integration (CI) and Continuous Deployment (CD) skills acquired during the training. To achieve this, I used this [template](https://github.com/ibm-developer-skills-network/vselh-ci-cd-final-project-template) provided by IBM Developer Skills Network.

# Topics

`Python 3.8`, `Flask`, `Microservices`, `REST API`, `CI/CD`, `GitHub Actions`, `Tekton`, `OpenShift`, `Nose`, `Pylint`, `MIT License`, `DevOps`, `IBM DevOps and Software Engineering Professional Certificate`

## Table of Contents
1. [Introduction](#introduction)
2. [Technologies Used](#technologies-used)
3. [Installation and Configuration](#installation-configuration)
4. [Usage](#usage)
5. [Development](#development)
6. [CI/CD Pipeline](#ci-cd-pipeline)
7. [Sources](#sources)
8. [License](#license)
9. [Contact](#contact)

## Introduction <a name="introduction"></a>

### Project Objective:
This project aims to create a microservice for managing and tracking counters, which is part of a larger system that includes a pre-built user interface (UI). The backend services will be integrated with Continuous Integration (CI) and Continuous Deployment (CD) pipelines using GitHub Actions and OpenShift Pipelines.

### Key Features:
- Create, read, update, delete, and list counters.
- Search for counters by name and value.

## Technologies Used <a name="technologies-used"></a>

### Programming Languages:
- Python 3.8

### Tools and Frameworks:
- Flask (for the REST API microservice)
- Nose (for testing)
- Pylint (for linting)
- Tekton (for CI/CD pipelines)
- OpenShift (for deployment)

## Installation and Configuration <a name="installation-configuration"></a>

### Prerequisites:
- Python 3.8
- pip (Python package manager)
- Docker (for containerization)
- OpenShift CLI (for deployment)

### Installation Steps:
1. Clone the GitHub repository:
    ```bash
    git clone https://github.com/ibm-developer-skills-network/vselh-ci-cd-final-project-template
    cd vselh-ci-cd-final-project-template
    ```

2. Create and activate a virtual environment:
    ```bash
    python3.8 -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3. Install the dependencies:
    ```bash
    pip install -r requirements.txt
    ```

### Environment Configuration:
Ensure the Python environment is properly set up with Python 3.8. Dependencies should be installed via pip.

### Environment Variables:
- `FLASK_ENV=development`

## Usage <a name="usage"></a>

### Usage Instructions:
To start the application, use the following command:
```bash
flask run
```
### Use Case Examples:
- **Create a counter:** 
    ```bash
    curl -X POST http://localhost:5000/counters -H "Content-Type: application/json" -d '{"name": "Sample Counter", "value": 10}'
    ```

- **Read a counter:**
    ```bash
    curl -X GET http://localhost:5000/counters/1
    ```

- **Update a counter:**
    ```bash
    curl -X PUT http://localhost:5000/counters/1 -H "Content-Type: application/json" -d '{"name": "Updated Counter", "value": 20}'
    ```

- **Delete a counter:**
    ```bash
    curl -X DELETE http://localhost:5000/counters/1
    ```

- **List counters by name:**
    ```bash
    curl -X GET http://localhost:5000/counters?name=Sample+Counter
    ```

## Development <a name="development"></a>

### Project Structure:
This backend project is primarily organized around the Flask framework. The directory structure includes the following folders:

- `.github/workflows/`: Contains the GitHub Actions workflows, specifically `workflow.yml`, which defines the CI pipeline for linting and testing.
- `tekton/`: Contains the Tekton tasks, specifically `tasks.yml`, which defines tasks for linting, testing, and building the image.
- `service/`: Contains the core of the microservice, specifically `routes.py`, which defines the API endpoints and their logic.
- `tests/`: Contains the unit tests for the microservice, specifically `test_routes.py`, which tests the functionality of the API endpoints.

```plaintext
.
├── .github/
│   └── workflows/
│       ├── workflow.yml
│       └── ...
├── tekton/
│   ├── tasks.yml
│   └── ...
├── service/
│   ├── __init__.py
│   ├── routes.py
│   └── ...
├── tests/
│   ├── test_routes.py
├── Dockerfile
├── requirements.txt
└── ....
```

## CI/CD Pipeline <a name="ci-cd-pipeline"></a>

### Continuous Integration (CI) with GitHub Actions

The codebase comes with unit tests for the provided endpoints. The goal is to set up CI pipelines using GitHub Actions to automate the linting and testing processes. To achieve this, we create a GitHub Actions workflow that triggers whenever changes are pushed to the repository.

Here is the content of the `github/workflows/workflow.yml` file:

```yaml
name: CI workflow

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    container: python:3.9-slim
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      - name: Lint with flake8
        run: |
          flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
      - name: Run unit tests with nose
        run: nosetests -v --with-spec --spec-color --with-coverage --cover-package=service
```

The workflow performs several steps:

- **Checkout:** Uses the GitHub Action to check out the repository.
- **Install dependencies:** Installs the necessary Python dependencies listed in `requirements.txt`.
- **Lint with flake8:** Runs `flake8` to ensure code quality and check for syntax errors.
- **Run unit tests with nose:** Executes unit tests using `nose` with detailed reporting and coverage information.

You can verify the success of the workflows by checking the "build passing" badge at the beginning of the README or by navigating to the "Actions" tab in the GitHub repository to see detailed execution logs.

### Continuous Delivery (CD) with Tekton and OpenShift Pipelines

In the second phase, we establish CD pipelines within OpenShift Pipelines. These pipelines should include steps for linting, testing, building an image, and deploying the microservice to an OpenShift cluster.

OpenShift is a platform that allows for the creation and management of CI/CD pipelines and the individual tasks corresponding to each step. It integrates with Tekton, an open-source framework enabling the definition and execution of build, test, and deployment pipelines within Kubernetes environments. OpenShift’s Tekton Task catalog provides pre-defined tasks that enhance productivity and allows for creating custom tasks for specific needs.

In this repository, Tekton automates the CD process, ensuring efficient and reliable builds and deployments. Two of the tasks required for the CD pipeline—cleanup and nose—have been created within this repository, while the remaining tasks were selected from the Tekton Task catalog available in OpenShift. This mix of custom and catalog tasks completes the pipeline setup on OpenShift.

Here is the content of the `.tekton/tasks.yml` file:

```yaml
---
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: cleanup
spec:
  description: This task will clean up a workspace by deleting all the files.
  workspaces:
    - name: source
  steps:
    - name: remove
      image: alpine:3
      env:
        - name: WORKSPACE_SOURCE_PATH
          value: $(workspaces.source.path)
      workingDir: $(workspaces.source.path)
      securityContext:
        runAsNonRoot: false
        runAsUser: 0
      script: |
        #!/usr/bin/env sh
        set -eu
        echo "Removing all files from ${WORKSPACE_SOURCE_PATH} ..."
        # Delete any existing contents of the directory if it exists.
        #
        # We don't just "rm -rf ${WORKSPACE_SOURCE_PATH}" because ${WORKSPACE_SOURCE_PATH} might be "/"
        # or the root of a mounted volume.
        if [ -d "${WORKSPACE_SOURCE_PATH}" ] ; then
          # Delete non-hidden files and directories
          rm -rf "${WORKSPACE_SOURCE_PATH:?}"/*
          # Delete files and directories starting with . but excluding ..
          rm -rf "${WORKSPACE_SOURCE_PATH}"/.[!.]*
          # Delete files and directories starting with .. plus any other character
          rm -rf "${WORKSPACE_SOURCE_PATH}"/..?*
---
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: nose
spec:
  workspaces:
    - name: source
  params:
    - name: args
      description: Arguments to pass to nose
      type: string
      default: "-v"
  steps:
    - name: nosetests
      image: python:3.9-slim
      workingDir: $(workspaces.source.path)
      script: |
          #!/bin/bash
          set -e
          python -m pip install --upgrade pip wheel
          pip install -r requirements.txt
          nosetests $(params.args)
```

The `.tekton/tasks.yml` file defines two tasks:

- **Cleanup Task:**
  - **Description:** Cleans up the workspace by deleting all files.
  - **Steps:** Uses the `alpine:3` image to perform the cleanup and deletes all files and directories in the specified workspace path, ensuring that no unwanted files remain.

- **Nose Task:**
  - **Description:** Runs unit tests using `nose`.
  - **Steps:** Uses the `python:3.9-slim` image, installs required Python packages, and executes `nosetests` with the specified arguments to run unit tests.

These tasks are essential for maintaining a clean workspace and ensuring that all tests are run correctly before deployment. Tekton's integration with OpenShift Pipelines allows for seamless execution of these tasks as part of the continuous delivery process.

### OpenShift Deployment
In this phase, we deploy the microservice using OpenShift Pipelines. This process ensures the application is built, tested, and deployed efficiently. Below are the key steps involved in deploying this repository via an OpenShift pipeline:

1. **Create a CI/CD workflow:** Set up a CI/CD workflow in OpenShift Pipelines to manage the build and deployment processes.
2. **Add parameters to tasks:** Define necessary parameters for the tasks created within the OpenShift Pipelines to ensure they run correctly.
3. **Add a workspace and persistent volume claim:** Configure a workspace and a persistent volume claim in the OpenShift UI to manage the storage required for the pipeline tasks.
4. **Add tasks:** Incorporate tasks to clone the GitHub repository, lint the source code, run unit tests, and deploy the application to the OpenShift cluster.

These steps ensure a seamless deployment process for the project. See below for screenshots illustrating the deployment process and its results.

## Sources <a name="sources"></a>

- **Template: [IBM Developer Skills Network - CI/CD Final Project Template](https://github.com/ibm-developer-skills-network/vselh-ci-cd-final-project-template)**

- **Useful links**:
  - **[Continuous Integration and Continuous Delivery (CI/CD)](https://www.coursera.org/learn/continuous-integration-and-continuous-delivery-ci-cd/home/week/1)**
  - **[IBM DevOps and Software Engineering Professional Certificate](https://www.coursera.org/professional-certificates/devops-and-software-engineering)**

## License <a name="license"></a>

This project is licensed under the MIT License - see the [LICENSE](/LICENSE) file for details.

## Contact <a name="contact"></a>

### Contact Information :

- Send me email : **fkanecloudtech@gmail.com**
- Connect with me on [LinkedIn](https://www.linkedin.com/in/fkanecloudtech/)
- Visit my [portfolio](https://fkanedev.github.io) to explore my projects and services.


### Contribution and Support :

Contributions are welcome. Please refer to the [CONTRIBUTING](/CONTRIBUTING.md) file for more information on how to contribute.
