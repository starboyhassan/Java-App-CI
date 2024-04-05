# Deploy Java application on AWS EKS cluster

## Overview
The project is a DevOps implementation for a Java application, utilizing various tools and services to automate the Continuous Integration (CI) and Continuous Deployment (CD) pipelines. The CI pipeline involves building and testing the application code, analyzing it with SonarQube, building a Docker image, and pushing it to AWS Elastic Container Registry (ECR). The CD pipeline updates Kubernetes deployment manifests with the new Docker image tag and triggers deployment using ArgoCD. Monitoring of the application is provided by Prometheus and Grafana.

![](https://github.com/starboyhassan/Java-App-CI/blob/main/digram/project_digram.png)

## Tools

- **GitHub**: Hosts the repositories containing application code and CI/CD pipeline definitions (Jenkinsfiles).
- **Ansible**: Used for installing and configuring Jenkins master, Jenkins agent, SonarQube, AWSCLI, EKSCTL, KubeCTL and Helm Charts tools on EC2 instances.
- **Jenkins**: Used for orchestrating the CI/CD pipelines and triggering builds upon GitHub repository commits. Jenkins also sends notifications to Slack upon pipeline success or failure.
- **Maven**: Utilized for building and testing the Java application.
- **SonarQube**: Performs static code analysis on the application code.
- **Docker**: Used for containerizing the application and building Docker images.
- **AWS ECR**: Used as the Docker image registry to store built images.
- **AWS EKS**: Used for Kubernate Cluster.
- **ArgoCD**: Used for continuous delivery to manage Kubernetes deployments.
- **kubectl**, **eksctl**: CLI tools for interacting with AWS EKS (Elastic Kubernetes Service) cluster.
- **Helm Charts**: Used to install ArgoCD and Prometheus and Grafana on the EKS cluster.
- **Prometheus** and **Grafana**: Used for monitoring the deployed application.
- **Slack**: Used for receiving notifications on pipeline success or failure.
---
## Project Structure

- **CI Repository**: Contains application files, Jenkinsfile, and Dockerfile. This repository triggers the CI pipeline upon commits. The CI pipeline performs Maven build, tests, SonarQube analysis, Docker image build, and pushes the image to AWS ECR.
- **CD Repository**: Contains deployment YAML files and Jenkinsfile. Upon successful completion of the CI pipeline, the CD pipeline is triggered. The CD pipeline updates Kubernetes deployment manifests with the new Docker image tag, pushes the changes to the CD repository, and triggers ArgoCD to sync the updated manifests with the Kubernetes cluster.

## Step-by-Step 

### 1. CI Pipeline:

1) Developer commits code to the GitHub CI repository.
2) GitHub triggers the Jenkins CI pipeline.
3) Jenkins CI pipeline checks out the CI repository.
4) Maven builds and tests the Java application.
5) SonarQube analyzes the code.
6) Docker builds the Docker image.
7) Docker image is pushed to AWS ECR.
8) Jenkins sends notifications to Slack upon CI pipeline success or failure.

### 2. CD Pipeline:
1) Upon successful completion of the CI pipeline, the CD pipeline is triggered.
2) Jenkins CD pipeline checks out the CD repository.
3) Deployment YAML files are updated with the new Docker image tag from ECR.
4) Updated deployment files are pushed to the CD repository.
5) Jenkins sends notifications to Slack upon CD pipeline success or failure.
6) ArgoCD syncs the updated deployment files with the Kubernetes cluster.
7) Prometheus and Grafana provide monitoring for the deployed application.

- For more details: [CD Repo](https://github.com/starboyhassan/Java-App-CD)

----
## Conclusion

#### This DevOps project successfully automates the CI/CD pipelines for a Java application. By utilizing Ansible for Configuarion management, Jenkins for pipeline orchestration, Docker for containerization, and Kubernetes with ArgoCD for deployment, the project ensures efficient development, testing, and deployment processes. Integration with SonarQube enables code quality analysis, while Prometheus and Grafana provide monitoring capabilities for the deployed application. Overall, this setup enhances development agility, improves deployment reliability, and ensures high-quality code delivery.




