AWS CI/CD Pipeline & EKS Deployment Project

Project Overview
This project demonstrates a fully automated CI/CD pipeline built entirely on AWS. It takes a pre-compiled React frontend application, containerizes it using an Nginx Docker image, pushes it to an AWS Elastic Container Registry (ECR), and automatically deploys it to an Amazon Elastic Kubernetes Service (EKS) cluster using AWS CodePipeline.

Architecture & Pipeline Explanation

The pipeline follows this continuous integration and deployment flow:
1. Source Code Management: The application code, Dockerfile, and Kubernetes manifests (manifest.yaml) are hosted in a GitHub repository.
2. Continuous Integration (AWS CodeBuild): Whenever a change is pushed to the main branch, AWS CodePipeline triggers AWS CodeBuild. CodeBuild acts as the factory worker, logging into AWS ECR, building a new Docker image, tagging it, and pushing it to the private ECR repository.
3. Continuous Deployment (AWS CodePipeline to EKS): Once the build succeeds, the native AWS CodePipeline "Deploy to EKS" action reads the `manifest.yaml` file and updates the Kubernetes pods with the newly pushed Docker image. 
4. Hosting (Amazon EKS): The application is hosted on a 2-node AWS EKS cluster (t3.medium instances). A Kubernetes Service of type `LoadBalancer` exposes the application to the internet on Port 3000.
5. Monitoring (CloudWatch): Execution logs from CodeBuild, deployment logs from the EKS Control Plane, and application pod logs are tracked and viewable in AWS CloudWatch.

Setup Instructions
To replicate this environment, the following infrastructure was provisioned via the AWS Console:
1. IAM Roles: - EKS-Cluster-Role for the Kubernetes Control Plane.
   - EKS-Node-Role for the EC2 worker nodes (with ECR read permissions).
2. Kubernetes Cluster:
   - Created an Amazon EKS Cluster named `brain-tasks-cluster`.
   - Provisioned a managed Node Group with two `t3.medium` instances.
3. Container Registry:
   - Created a private AWS ECR repository to store the Nginx/React Docker images.
4. CI/CD Automation:
   - Created an AWS CodeBuild project (`brain-tasks-build`) running in privileged mode to build Docker images.
   - Created an AWS CodePipeline (`brain-tasks-pipeline`) linking GitHub (V2), CodeBuild, and Amazon EKS.
   - Configured an EKS Access Entry granting the CodePipeline Service Role `AmazonEKSClusterAdminPolicy` so it could deploy to the cluster.

Project Screenshots
1. Live Application

<img width="1917" height="1034" alt="image" src="https://github.com/user-attachments/assets/0a7c4f36-978b-47d7-bbe0-d3a3fcbe70ae" />

2. Successful CI/CD Pipeline

<img width="1908" height="1032" alt="CI-CD-Pipeline" src="https://github.com/user-attachments/assets/1464d34b-ddd4-417c-9d3d-555486bb1acb" />

3. Monitoring & Logs (CloudWatch)

<img width="1918" height="1031" alt="Code Build Logs" src="https://github.com/user-attachments/assets/6e34113b-acef-4df2-a24f-0feddff85264" />

<img width="1910" height="1028" alt="EKS cloud watch logs" src="https://github.com/user-attachments/assets/119cb21b-387a-4690-b291-12a70e8faf4c" />

4. Kubernetes Pods & Logs

<img width="1918" height="1078" alt="kubectl pods, svc and pod logs" src="https://github.com/user-attachments/assets/4a764283-48db-47be-a12e-018b52049efb" />

5. EKS cluster and Node Group

   <img width="1918" height="1035" alt="EKS cluster" src="https://github.com/user-attachments/assets/0d043f96-7915-41e5-9734-e389d2b4c408" />
   <img width="1916" height="1033" alt="EKS Node groups" src="https://github.com/user-attachments/assets/06986bce-e21a-4a19-86b1-1a1045294aca" />

6. AWS code build
   
<img width="1916" height="1038" alt="AWS Code Build" src="https://github.com/user-attachments/assets/886385a3-2b1d-4585-97c5-0a9af92fe6e8" />

7. ECR Private Repo
   
<img width="1916" height="1037" alt="ECR private Repo" src="https://github.com/user-attachments/assets/3d22e028-b7dd-4c05-bd55-1e379c4bcce2" />

   




