# Tekton Create a baseline Pipeline Lab

## Overview
This lab demonstrates how to create and manage a continuous deployment (CD) pipeline using Tekton. I defined tasks and pipelines to automate the process of cloning a repository, linting code, running tests, building an image, and deploying the application.

## Objectives
- Create Tekton tasks to perform specific CI/CD operations.
- Assemble these tasks into a Tekton pipeline.
- Understand how to manage and execute a CI/CD pipeline using Tekton.

## Prerequisites
- Kubernetes cluster with Tekton installed.
- `kubectl` configured to interact with your Kubernetes cluster.

## Tasks Description

### Echo Task
The Echo task uses an Alpine Linux container to print a specified message. It is a generic task that can be reused to log messages at different stages of the pipeline.

### Checkout Task
The Checkout task uses a Git container to clone a specified Git repository and branch. This task is essential for pulling the latest code changes before performing any further CI/CD operations.

### Lint Task
The Lint task runs a linting tool, such as Flake8, to check the code quality and ensure it adheres to the specified coding standards. This step helps in maintaining code quality by catching syntax and style issues early in the pipeline.

### Test Task
The Test task runs unit tests using a framework like PyUnit. It ensures that the code changes do not break existing functionality and that the new features work as expected.

### Build Task
The Build task creates a container image from the source code. It uses the Dockerfile in the repository to build the image and prepares it for deployment.

### Deploy Task
The Deploy task deploys the newly built container image to a specified environment. It ensures that the latest changes are available in the target environment for further testing or production use.

## Steps to Execute the Pipeline

1. **Apply Tasks:** Applied the task definitions to the Kubernetes cluster.
    ```sh
    kubectl apply -f echo-task.yaml
    kubectl apply -f checkout-task.yaml
    ```

2. **Apply Pipeline:** Applied the pipeline definition to the Kubernetes cluster.
    ```sh
    kubectl apply -f cd-pipeline.yaml
    ```

3. **Run the Pipeline:** Created a PipelineRun to execute the pipeline.
    ```yaml
    apiVersion: tekton.dev/v1beta1
    kind: PipelineRun
    metadata:
        name: cd-pipeline-run
    spec:
        pipelineRef:
            name: cd-pipeline
        params:
            - name: repo-url
              value: "https://github.com/your-repo.git"
            - name: branch
              value: "main"
    ```
    Applied the PipelineRun:
    ```sh
    kubectl apply -f cd-pipeline-run.yaml
    ```

4. **Monitor the Pipeline:** Checked the status of the PipelineRun.
    ```sh
    tkn pipelinerun logs cd-pipeline-run -f
    ```

## Summary
In this lab, I created Tekton tasks and a pipeline to automate a CI/CD workflow. I defined tasks for cloning a repository, linting, testing, building, and deploying an application, and orchestrated these tasks using a Tekton pipeline.
