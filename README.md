# Udacity Capstone Cloud DevOps Nanodegree

## Introduction

This project is a requirement to graduate at [Cloud DevOps Nanodegree](https://www.udacity.com/course/cloud-dev-ops-nanodegree--nd9991). The project requires to set up a Kubernetes cluster hosted at AWS EKS, and deploy the Docker image using Jenkins Pipeline.

## Technologies

* [Bcrypt Sandbox](https://github.com/felladrin/bcrypt-sandbox), as the main application.
* Docker, for building a ready-deploy application.
* Kubernetes(AWS EKS), for automating deployment, scaling, and management of containerized applications.
* NGINX Ingress Controller, for assembling an NGINX configuration file (nginx.conf).
* CloudFormation, for stack creation process automation.
* Aqua Microscanner, for docker protection, monitoring, logging and real-time analysis.
* Jenkins, for automatic integrations and deployments(CI/CD).

## Prerquisites 
* [AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=default&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)
* [IAM User](https://aws.amazon.com/iam/)
* [AWS CLI](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
* [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
* [Docker](https://docs.docker.com/get-docker/)

## Creating stacks
  - Network Template~~ `./create-stack.sh "your-stack-name" cp-vpc-config.yml cp-vpc-params.json`
  - EKS Cluster Template~~ `./create-stack.sh "your-stack-name" cp-eks-cluster.yml cp-vpc-params.json`
  - Jenkins Template~~ `./create-stack.sh "your-stack-name" cp-jenkins.yml cp-vpc-params.json`
  - Cluster NodeGroups Template~~ `./create-stack.sh "your-stack-name" cp-cluster-nodegroup.yml cp-vpc-params.json`

## Files
* Dockerfile
* Jenkinsfile
* Kubernetes Deployment Template~~
* Source Code
