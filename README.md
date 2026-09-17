# Coworking Analytics on AWS EKS

A lightweight, cloud-native deployment for a Flask-based analytics service and PostgreSQL database running on Amazon EKS.

## Overview

This project demonstrates a production-minded microservice deployment using Docker, Kubernetes, and AWS services. The analytics application is containerized with Docker, stored in Amazon ECR, and deployed to Amazon EKS. PostgreSQL is deployed within the Kubernetes cluster using persistent storage.

The solution includes:

- Flask-based analytics application
- PostgreSQL with persistent storage
- Amazon ECR for container image storage
- AWS CodeBuild for CI/CD automation
- Kubernetes ConfigMap and Secret resources
- Readiness and liveness health checks
- Automated GitHub-triggered builds

## Architecture

The deployment workflow follows:

```text
GitHub → AWS CodeBuild → Amazon ECR → Amazon EKS
```

Within the Kubernetes cluster:

```text
Analytics Pod → PostgreSQL Service → PostgreSQL Pod
```

Configuration data is stored separately from sensitive credentials using Kubernetes ConfigMaps and Secrets.

## Key Components

### Application

- Flask analytics microservice
- Dockerized and stored in Amazon ECR
- Deployed using Kubernetes Deployments
- Exposed through a Kubernetes LoadBalancer Service

### Database

- PostgreSQL running in Kubernetes
- Persistent storage through Persistent Volumes (PV) and Persistent Volume Claims (PVC)
- Internal access through a Kubernetes service

### Configuration and Security

- ConfigMap stores non-sensitive runtime configuration
- Secret stores database credentials
- Liveness and readiness probes ensure service health and availability

## Deployment Workflow

1. Code is committed and pushed to GitHub.
2. A GitHub webhook automatically triggers AWS CodeBuild.
3. CodeBuild builds the Docker image using `buildspec.yaml`.
4. The image is pushed to Amazon ECR.
5. Kubernetes pulls the latest image from ECR.
6. The application is deployed and connected to PostgreSQL through the Kubernetes service layer.

## Recommended Infrastructure

A `t3.medium` worker node provides a good balance of cost and performance for this workload. It offers sufficient CPU and memory for both the analytics service and PostgreSQL while remaining economical for development and small-scale deployments.

## Cost Optimization

- Use a single worker node for development environments.
- Delete unused EKS clusters after project completion.
- Remove unused ECR images and repositories.
- Delete unused EBS volumes and AWS Load Balancers.
- Regularly clean up unused cloud resources to avoid unexpected charges.

## Why This Design Works

This architecture combines containerization, Kubernetes orchestration, and AWS-native services to provide a repeatable deployment process. Automated builds, centralized image storage, infrastructure as code, and Kubernetes health probes improve reliability while keeping the solution simple and easy to maintain.