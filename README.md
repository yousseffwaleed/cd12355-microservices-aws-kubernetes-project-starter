# Coworking Analytics Deployment

This project deploys a Flask-based analytics microservice and a PostgreSQL database on Amazon EKS using Kubernetes.

The analytics application is containerized with Docker and stored in Amazon Elastic Container Registry (ECR).

AWS CodeBuild serves as the CI pipeline and automatically builds Docker images from the GitHub repository using the buildspec.yaml configuration.

Successful builds push versioned application images to ECR, making them available for Kubernetes deployments.

The PostgreSQL database runs as a Kubernetes deployment with persistent storage provided through a Persistent Volume (PV) and Persistent Volume Claim (PVC).

Application configuration is managed through a Kubernetes ConfigMap.

Sensitive values such as database credentials are stored in a Kubernetes Secret.

The analytics application is deployed using a Kubernetes Deployment and exposed through a LoadBalancer Service.

Readiness and liveness probes are configured to ensure the application remains healthy and available.

The application connects to PostgreSQL through an internal Kubernetes service rather than direct pod communication.

Deployment updates are performed by building a new Docker image, pushing it to ECR, and updating or restarting the Kubernetes deployment.

The CI/CD workflow follows the pattern: GitHub → CodeBuild → ECR → EKS.

## Architecture

GitHub → CodeBuild → Amazon ECR → Amazon EKS

Analytics Pod → PostgreSQL Service → PostgreSQL Pod

ConfigMap and Secret resources provide runtime configuration and credentials to the application.

## Recommended Instance Type

A t3.medium worker node is appropriate for this workload because the analytics service and PostgreSQL database have modest CPU and memory requirements while remaining cost-effective.

## Cost Optimization

Costs can be reduced by deleting EKS clusters, load balancers, ECR repositories, and EBS volumes when the environment is not needed.

Using a single-node development cluster and removing unused Docker images further reduces expenses.