# Brain Tasks App - DevOps Deployment Project

## Project Overview

This project demonstrates a complete DevOps deployment workflow for a React static application using:

- Docker
- DockerHub
- Kubernetes
- AWS EKS
- AWS CodeBuild
- AWS CodePipeline
- CloudWatch

The application was provided as a production-ready `dist/` folder.

---

# Application Architecture

GitHub Repo
↓
CodePipeline
↓
CodeBuild
↓
DockerHub
↓
AWS EKS Cluster
↓
Kubernetes Deployment + Service
↓
AWS LoadBalancer
↓
Application Access

---

# Technologies Used

- Docker
- DockerHub
- Kubernetes
- AWS EKS
- AWS CodeBuild
- AWS CodePipeline
- AWS CloudWatch
- Nginx
- EC2 Ubuntu

---

# Docker Setup

## Build Docker Image

```bash
docker build -t brain-tasks-app .
