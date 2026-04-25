# AWS CI/CD Capstone Project

This repository contains a sample Node.js application and all necessary AWS configuration files for implementing a complete CI/CD pipeline using AWS CodePipeline, CodeBuild, CodeDeploy, ECR, and ECS.

---

## Project Overview

This project demonstrates how to design and implement a fully automated CI/CD pipeline on AWS that takes code from GitHub, builds and tests it, packages it into a Docker container, and deploys it to an Amazon ECS cluster behind a load balancer. The pipeline integrates CodePipeline, CodeBuild, CodeDeploy, and ECR, along with monitoring and approval stages.

---

## Architecture

| Stage | Service | Description |
|---|---|---|
| **Source** | GitHub | Repository containing a Node.js web application |
| **Build** | AWS CodeBuild | Compiles the app, runs unit tests, and builds a Docker image |
| **Artifact Storage** | Amazon ECR | Stores the built Docker image |
| **Deploy** | AWS CodeDeploy | Deploys the new container to Amazon ECS (Fargate) |
| **Orchestration** | AWS CodePipeline | Ties all pipeline stages together |
| **Monitoring** | CloudWatch | Alarms monitor pipeline and ECS health |
| **Approval** | Manual Gate | Manual approval stage before production deployment |

---

## Repository Structure

```
myapp/
├── src/
│   ├── app.js
│   ├── package.json
│   └── package-lock.json
├── tests/
│   └── app.test.js
├── Dockerfile
├── buildspec.yml
├── appspec.yml
├── taskdef.json
└── README.md
```

---

## File Descriptions

### `src/app.js`
A simple Express.js application that responds with a message.

### `src/package.json`
Defines dependencies for the Node.js application.

### `tests/app.test.js`
Simple Jest test to verify the application works.

### `Dockerfile`
Containerizes the application using Node.js 16.

### `buildspec.yml`
Instructions for CodeBuild to:
- Install dependencies
- Run tests
- Build a Docker image
- Push the image to Amazon ECR
- Generate `imagedefinitions.json` for CodePipeline

### `appspec.yml`
Defines how CodeDeploy should update the ECS service.

### `taskdef.json`
Template for the ECS task definition that defines the container configuration.

---

## CI/CD Pipeline Flow

1. **Source Stage** — CodePipeline fetches source code from GitHub
2. **Build Stage** — CodeBuild runs tests, builds Docker image, and pushes to ECR
3. **Deploy Stage** — CodeDeploy updates ECS service with the new image
4. **Approval Stage** — Manual approval before production deployment
5. **Monitoring** — CloudWatch alarms monitor pipeline and ECS health

---

## Setup Instructions

1. Replace `<account_id>` and `<TASK_DEFINITION>` placeholders in the configuration files
2. Create the ECR repository named `myapp-repo`
3. Create the ECS cluster and service with Fargate launch type
4. Set up an Application Load Balancer to route traffic
5. Connect GitHub repository to CodePipeline
6. Configure CodeBuild with the `buildspec.yml` file
7. Configure CodeDeploy with the `appspec.yml` and `taskdef.json` files
8. Set up CloudWatch alarms and SNS notifications for monitoring

---

## How It Works

Once set up, every commit to the GitHub repository will automatically trigger the CI/CD pipeline:

- **CodePipeline** fetches the latest code
- **CodeBuild** runs tests and builds a Docker image
- The Docker image is pushed to **ECR**
- **CodeDeploy** updates the ECS service with the new image
- The application is served behind the **Application Load Balancer**
