# aws-container-workshop

## Agenda
* EKS Overview
* Launch EKS cluster with eksctl
* Deploy Microservices to EKS Fargate
* ECS Overview
* Deploy Microservices to ECS Fargate
* Python CDK hand-on

## Amazon EKS Workshop
### Please open aws eks workshop to finish action as following (https://eksworkshop.com/)
* Introduction
  * Kubernetes (k8s) Basics
  * Kubernetes Architecture
* Start the workshop
  * Install Kubernetes Tools
  * IAM Settings For Your Workspace
  * Create An ssh Key
  * Create An Aws Kms Custom Managed Key (cmk)
* Launch Using eksctl
* Begineer
  * Deploying Microservices To EKS fargate

## Amazon ECS Workshop
### Please open aws ecs workshop to finish action as following (https://ecsworkshop.com/)
* Introduction
  * ECS Overview

### Please open aws console to finish action as following (https://console.aws.amazon.com/ecs)
* Amazon ECS > Account Settings: Enable CloudWatch Container Insights
* Amazon ECS > Clusters > Get Started
  * Step 1: Container and Task > Container definition > Custom > configure
    * Image: lexwhen/docker-2048
    * Port mappings > Container port: 80
  * Step 2: Service > Define your service > Edit
    * Number of desired tasks > 3
    * Elastic Load Balancing (optional) > Load balancer type > Application Load Balancer
  * Step 3: Cluster
  * Step 4: Review


## AWS CDK Workshop Python
### Please open aws cdk workshop to finish action as following (https://cdkworkshop.com/)
* AWS CDK Intro Workshop
  * Python Workshop
    * New Project
    * Hellow CDK!

### Deploy ECS Cluster with Python CDK 
* Creating the directory and initializing the AWS CDK
```
mkdir MyEcsConstruct
cd MyEcsConstruct
cdk init --language python
source .env/bin/activate
pip install -r requirements.txt
```
* Add the Amazon EC2 and Amazon ECS packages
```
pip install aws_cdk.aws_ec2 aws_cdk.aws_ecs aws_cdk.aws_ecs_patterns
```
* Create a Fargate service: Please open the dir you created my_ecs_construct > my_ecs_construct_stack.py and update import package
```
from aws_cdk import (core, aws_ec2 as ec2, aws_ecs as ecs,
                     aws_ecs_patterns as ecs_patterns)
```
* Replace the comment at the end of the constructor with the following code.
```
vpc = ec2.Vpc(self, "MyVpc", max_azs=3)     # default is all AZs in region

cluster = ecs.Cluster(self, "MyCluster", vpc=vpc)

ecs_patterns.ApplicationLoadBalancedFargateService(self, "MyFargateService",
    cluster=cluster,            # Required
    cpu=512,                    # Default is 256
    desired_count=6,            # Default is 1
    task_image_options=ecs_patterns.ApplicationLoadBalancedTaskImageOptions(
        image=ecs.ContainerImage.from_registry("amazon/amazon-ecs-sample")),
    memory_limit_mib=2048,      # Default is 512
    public_load_balancer=True)  # Default is False
```
* Save the file my_ecs_construct_stack.py and show different of stack.
```
cdk diff
```
* Deploy the stack.
```
cdk deploy
```
* Clear up
```
cdk destroy
```

