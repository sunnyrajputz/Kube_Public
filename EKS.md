# What is EKS?

Amazon Elastic Kubernetes Service (EKS) is a fully managed Kubernetes service provided by Amazon Web Services (AWS). It makes it easier to deploy, manage, and scale containerized applications using Kubernetes on AWS. EKS automatically handles the availability and scalability of the Kubernetes control plane, including patching and upgrades. It integrates with various AWS services, making it a powerful option for managing containerized applications in the cloud.

# Components of EKS
EKS consists of the following key components:
# EKS Control Plane:
The managed Kubernetes control plane includes the Kubernetes master nodes responsible for cluster management (e.g., API server, controller manager, etc.). AWS manages this control plane, ensuring its availability and scaling.
# Worker Nodes:
These are EC2 instances that run your containerized applications. Worker nodes are managed by the Kubernetes control plane and run your Pods (the smallest deployable units in Kubernetes).
# Amazon EC2 Instances:
Worker nodes are typically EC2 instances running within your VPC. These instances host the containers running your applications.
# VPC (Virtual Private Cloud):
EKS clusters are deployed in a VPC, which provides network isolation. The VPC defines the network boundaries in which the worker nodes and other components reside.
# IAM (Identity and Access Management):
EKS uses IAM for user and service access management. IAM roles and policies are used to control who can access and manage the Kubernetes cluster and its components.
# Amazon ECR (Elastic Container Registry):
ECR is often used to store container images that are pulled into EKS for deployment. You can use your private registry to manage container images that EKS uses.
# CloudWatch:
CloudWatch is used for logging and monitoring Kubernetes clusters and applications running on EKS. It helps in observability and troubleshooting.
# AWS Auto Scaling:
EKS integrates with AWS Auto Scaling to dynamically scale the number of EC2 instances (worker nodes) based on demand.

# How EKS Works
EKS Cluster Setup:
You create an EKS cluster in the AWS Management Console, specifying the VPC, subnets, and other configurations.
EKS sets up a managed Kubernetes control plane (master nodes), which is highly available and automatically scaled.
Worker Nodes Setup:
You configure worker nodes (EC2 instances) that will run your containerized applications.
These nodes are registered with the Kubernetes control plane. EKS manages the lifecycle of these nodes.
Deploying Applications:
After the cluster is set up, you can deploy containerized applications to the cluster using standard Kubernetes deployment tools (kubectl, Helm, etc.).
The Kubernetes scheduler deploys these applications across the available worker nodes.
Cluster Management:
EKS automatically manages the Kubernetes control plane, such as scaling, patching, and ensuring high availability.
You can manage the Kubernetes workloads through the Kubernetes API, AWS CLI, or other management tools.

