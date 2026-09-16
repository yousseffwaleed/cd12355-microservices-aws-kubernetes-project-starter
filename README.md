# Coworking Analytics on AWS EKS

A lightweight, cloud-native deployment for a Flask-based analytics service and PostgreSQL database running on Amazon EKS.

## Overview

This project demonstrates a simple but production-minded microservice setup for coworking analytics. It packages the Python application in Docker, stores images in Amazon ECR, and deploys the application and database to Kubernetes on AWS.

The solution includes:

- A containerized Flask analytics app
- PostgreSQL running in Kubernetes with persistent storage
- CI/CD automation with AWS CodeBuild
- Environment configuration via ConfigMap and Secret
- Health checks and service-based connectivity
- Clean deployment flow from GitHub to EKS

## Architecture

The deployment follows this flow:

GitHub → AWS CodeBuild → Amazon ECR → Amazon EKS

Within the cluster:

Analytics Pod → PostgreSQL Service → PostgreSQL Pod

This keeps application configuration separate from sensitive credentials while ensuring the service can access the database reliably through Kubernetes networking.

## Key Components

### Application
- Flask-based analytics microservice
- Packaged into a Docker image
- Deployed as a Kubernetes Deployment
- Exposed through a LoadBalancer Service

### Database
- PostgreSQL deployed as a Kubernetes workload
- Persistent storage via Persistent Volume (PV) and Persistent Volume Claim (PVC)
- Accessed through an internal Kubernetes service

### Configuration and Security
- Kubernetes ConfigMap handles non-sensitive runtime settings
- Kubernetes Secret stores credentials and sensitive values
- Readiness and liveness probes keep the app healthy and resilient

## Deployment Workflow

1. Code is pushed to GitHub.
2. AWS CodeBuild builds the Docker image from the repository.
3. The built image is pushed to Amazon ECR.
4. Kubernetes pulls the latest image and deploys the updated app.
5. The app connects to PostgreSQL through the internal service layer.

## Recommended Infrastructure

A t3.medium worker node is a good fit for this workload. It balances cost and performance for a modest analytics service and PostgreSQL database in a development or small production setup.

## Cost Optimization

To keep cloud spend under control:

- Delete unused EKS clusters when they are no longer needed
- Remove load balancers, ECR repositories, and EBS volumes after teardown
- Use a single-node cluster for development environments
- Clean up unused Docker images and stale artifacts

## Why This Setup Works

This project combines Kubernetes, containerization, and AWS-native tooling to create a practical deployment pattern that is easy to extend. It is well suited for learning, prototyping, and small-scale cloud workloads where reliability and repeatable deployment matter.

---

Built for a simple, scalable, and cloud-ready coworking analytics platform.