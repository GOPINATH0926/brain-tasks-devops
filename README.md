# Brain Tasks App - DevOps Deployment Project

## Project Overview

This project demonstrates a complete DevOps deployment workflow for a React static application using:

- Docker
- DockerHub
- Kubernetes
- AWS EKS
- AWS CodeBuild
- AWS CodePipeline
- AWS CloudWatch

The application was provided as a production-ready `dist/` folder.

---

# Application Architecture

GitHub Repository  
↓  
AWS CodePipeline  
↓  
AWS CodeBuild  
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
- Ubuntu EC2

---

# Step 1 - Clone Repository

```bash
git clone https://github.com/Vennilavanguvi/Brain-Tasks-App.git
cd Brain-Tasks-App
Step 2 - Docker Setup
Dockerfile
FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*

COPY dist/ /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
Build Docker Image
docker build -t brain-tasks-app .
Run Docker Container
docker run -d -p 3000:80 --name brain-tasks brain-tasks-app
Verify Running Container
docker ps
Step 3 - DockerHub Setup
Login to DockerHub
docker login
Tag Docker Image
docker tag brain-tasks-app gopinathsiva2605/brain-tasks-app:latest
Push Docker Image
docker push gopinathsiva2605/brain-tasks-app:latest
DockerHub Repository

https://hub.docker.com/r/gopinathsiva2605/brain-tasks-app

Step 4 - Kubernetes Setup using AWS EKS
Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl

sudo mv kubectl /usr/local/bin/
Install eksctl
curl --silent --location "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin/
Configure AWS CLI
aws configure
Create EKS Cluster
eksctl create cluster \
  --name brain-tasks-cluster \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 2 \
  --managed
Verify Cluster Nodes
kubectl get nodes
Step 5 - Kubernetes Deployment
deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: brain-tasks-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: brain-tasks

  template:
    metadata:
      labels:
        app: brain-tasks

    spec:
      containers:
      - name: brain-tasks

        image: gopinathsiva2605/brain-tasks-app:latest

        ports:
        - containerPort: 80
service.yaml
apiVersion: v1
kind: Service

metadata:
  name: brain-tasks-service

spec:
  type: LoadBalancer

  selector:
    app: brain-tasks

  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
Deploy Application to Kubernetes
kubectl apply -f deployment.yaml

kubectl apply -f service.yaml
Verify Pods
kubectl get pods
Verify Services
kubectl get svc
LoadBalancer URL
http://a65f61c4fd7374b67bb395ccb3ca4ffc-18292735.us-east-1.elb.amazonaws.com
Step 6 - AWS CodeBuild
buildspec.yml
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Docker Hub
      - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

  build:
    commands:
      - echo Building Docker image
      - docker build -t brain-tasks-app .
      - docker tag brain-tasks-app gopinathsiva2605/brain-tasks-app:latest

  post_build:
    commands:
      - echo Pushing Docker image
      - docker push gopinathsiva2605/brain-tasks-app:latest
CodeBuild Features
Automatically builds Docker image
Automatically pushes image to DockerHub
Connected with GitHub repository
Step 7 - AWS CodePipeline
Pipeline Stages
Source Stage → GitHub
Build Stage → AWS CodeBuild
Pipeline Workflow

GitHub Push
↓
CodePipeline Triggered
↓
CodeBuild Starts
↓
Docker Image Built
↓
Docker Image Pushed to DockerHub

Step 8 - Monitoring using CloudWatch

CloudWatch Logs were used to monitor:

CodeBuild logs
CodePipeline execution logs
Kubernetes application logs
Useful Kubernetes Commands
Check Pods
kubectl get pods
Check Services
kubectl get svc
Check Deployment
kubectl get deployment
View Pod Logs
kubectl logs brain-tasks-deployment-675cf759dd-5mkt5
Project Output

Successfully deployed the Brain Tasks application using:

Docker containerization
Kubernetes orchestration
AWS EKS cluster
CI/CD pipeline using CodeBuild and CodePipeline
GitHub Repository

https://github.com/GOPINATH0926/brain-tasks-devops

Author

Gopinath
