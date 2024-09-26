# aws_eks
aws_eks

To set up **Amazon Elastic Kubernetes Service (EKS)** in AWS, several prerequisites are needed to ensure a smooth setup. Here's a breakdown of these prerequisites:

### 1. **AWS Account**
   - Ensure you have an active AWS account with the necessary permissions to create and manage EKS clusters.

### 2. **AWS CLI and EKS CLI Tools**
   - **AWS CLI**: Install the AWS Command Line Interface (CLI) to interact with AWS resources.
   - **kubectl**: The Kubernetes CLI (`kubectl`) is needed to interact with your EKS cluster.
   - **eksctl**: A command-line tool for simplifying the process of creating and managing EKS clusters.

   **Installation:**
   - AWS CLI: Install from [here](https://aws.amazon.com/cli/).
   - kubectl: Install from [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
   - eksctl: Install from [here](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html).

### 3. **IAM Permissions**
   - You need an IAM user/role with the following permissions:
     - `eks:CreateCluster`, `eks:DescribeCluster`, `eks:ListClusters`
     - EC2-related permissions (e.g., `ec2:CreateSecurityGroup`, `ec2:CreateVpc`, `ec2:CreateSubnet`)
     - `cloudformation:*` (if using CloudFormation)
     - `iam:CreateRole`, `iam:AttachRolePolicy` (for worker node IAM roles)

   Use AWS-managed policies like `AmazonEKSClusterPolicy`, `AmazonEKSServicePolicy`, and `AmazonEKSWorkerNodePolicy` as a base.

### 4. **VPC and Networking Requirements**
   - **VPC**: You need a VPC with subnets to host your EKS cluster. Ensure the following:
     - At least one public and one private subnet in different Availability Zones.
     - Subnets must have internet access (through an Internet Gateway for public subnets or NAT gateway for private subnets).
   - **Security Groups**: You'll need to configure security groups that allow traffic between the control plane and the worker nodes.
   
   You can create a VPC manually or let `eksctl` create it automatically.

### 5. **IAM Roles**
   - **Cluster Role**: An IAM role that EKS will assume for managing Kubernetes control plane resources. Attach the `AmazonEKSClusterPolicy`.
   - **Worker Node Role**: The role that EC2 instances (nodes) in your cluster will use. Attach the `AmazonEKSWorkerNodePolicy`, `AmazonEC2ContainerRegistryReadOnly`, and `AmazonEKS_CNI_Policy`.

### 6. **EC2 Key Pair (Optional but recommended)**
   - If you want SSH access to your worker nodes, create an EC2 key pair.

### 7. **Amazon ECR (Optional)**
   - If you want to use Amazon Elastic Container Registry (ECR) for storing your container images, make sure it's set up, and your IAM role has access to it.

### 8. **Helm (Optional)**
   - Helm is a Kubernetes package manager that simplifies installing applications on Kubernetes. Install Helm if you plan to deploy apps using Helm charts.

### 9. **Enable Kubernetes Dashboard (Optional)**
   - You can optionally enable the Kubernetes Dashboard for a visual interface to manage Kubernetes resources. This can be done after cluster setup.

### 10. **Resource Quotas**
   - Make sure your AWS account has the required service quotas, such as sufficient EC2 instances, VPCs, subnets, etc.

---

### Steps to Set Up EKS

Once the prerequisites are met, follow these basic steps:

1. **Create the EKS Cluster**:
   - Using `eksctl`: 
     ```bash
     eksctl create cluster --name my-cluster --region <region> --nodegroup-name my-nodes --node-type t3.medium --nodes 2
     ```

2. **Configure `kubectl`**:
   - Update your `kubectl` configuration to use the EKS cluster:
     ```bash
     aws eks --region <region> update-kubeconfig --name my-cluster
     ```

3. **Deploy Worker Nodes**:
   - Using CloudFormation or `eksctl`, deploy the worker nodes into the cluster.

4. **Test the Cluster**:
   - Verify the cluster is running by listing nodes:
    
     kubectl get nodes
