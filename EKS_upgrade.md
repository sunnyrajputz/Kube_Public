# Components Required for EKS Cluster Version Upgrade
To upgrade your EKS cluster version, several components must be considered:
EKS Control Plane Version:
AWS manages the version of the EKS control plane. This is what you typically upgrade to take advantage of new Kubernetes features, security patches, and bug fixes.
Kubernetes Node Version:
The worker nodes running your applications must be compatible with the new control plane version. You need to upgrade the node AMI (Amazon Machine Image) to match the Kubernetes version you are upgrading to.
Node Group Configuration:
EKS supports managed node groups. When upgrading the cluster version, you might also need to upgrade your managed node groups to use the appropriate Kubernetes version.
Add-ons (e.g., CoreDNS, kube-proxy):
EKS uses add-ons for various services like CoreDNS, kube-proxy, etc. These should be updated to be compatible with the new Kubernetes version.
Helm and Other Third-Party Applications:
If you’re using Helm or other third-party applications, ensure they are compatible with the new Kubernetes version.

How to Upgrade an EKS Cluster Version Without Downtime
Upgrading your EKS cluster version is a careful process to avoid downtime. Follow these steps for an online upgrade with minimal disruption:
Pre-Upgrade Preparations:
Backup your workloads: Always take a backup of your applications, configurations, and persistent data.
Test on a Staging Cluster: If possible, perform the upgrade on a staging cluster first to validate the process and ensure that your applications are compatible with the new version.
Check Compatibility: Ensure that your node group AMIs, Kubernetes workloads, Helm charts, and any add-ons (e.g., CoreDNS, kube-proxy) are compatible with the version you're upgrading to.
Upgrade the EKS Control Plane:
From the AWS Management Console or CLI, initiate the upgrade of the EKS control plane to the desired version.
AWS will automatically upgrade the control plane and maintain its availability during the process.
Upgrade Node Groups:
Managed Node Groups: EKS allows you to upgrade the node group AMI via the console or CLI. Create a new node group with the updated AMI corresponding to the new Kubernetes version.
Self-Managed Node Groups: If you're using self-managed node groups, you'll need to manually upgrade the EC2 instances running your worker nodes. You can do this by updating the EC2 instance's AMI and gradually rolling out new nodes while draining and terminating old ones.
Pod Disruption Management:
Ensure you have PodDisruptionBudgets (PDBs) set up for your applications. This ensures that the required number of replicas of your pods are available during the upgrade and that disruptions are minimal.
Kubernetes will automatically reschedule pods on the upgraded nodes without downtime.
Drain and Replace Old Nodes:
After upgrading the control plane and creating new worker nodes, drain the old worker nodes (using kubectl drain) to safely evict the pods from the old nodes and reschedule them onto the new nodes.
Terminate the old nodes after draining them, and the new nodes will take over.
Verify the Cluster and Workloads:
Check that the upgraded cluster is operating smoothly, and verify that all workloads are functioning as expected.
Update Add-ons:
After upgrading the control plane and worker nodes, ensure that any add-ons, such as CoreDNS and kube-proxy, are also updated to versions compatible with the new Kubernetes version.

By following these steps, you can ensure a smooth upgrade process for your EKS cluster, minimizing downtime and keeping your applications available during the upgrade.



Key Steps:
Upgrade the EKS control plane (master nodes).
Upgrade the worker nodes (EC2 instances).
Ensure workloads are rescheduled and running without disruptions.
Step-by-Step Process
1. Check the current EKS Cluster Version
Before performing any upgrade, you should verify the current version of your cluster.
```
aws eks describe-cluster --name <cluster-name> --query "cluster.version"
```


3. Choose the new EKS Cluster Version
Find the available versions for your EKS cluster by checking the EKS release notes or using AWS CLI to list the available Kubernetes versions.
```
aws eks describe-cluster --name <cluster-name> --query "cluster.version"
aws eks list-clusters
```
4. Upgrade the EKS Control Plane (Master Nodes)
To upgrade the control plane to a new Kubernetes version, you will use the following AWS CLI command. This step is non-disruptive because AWS automatically handles the process for you.
bash
Copy
aws eks update-cluster-version --name <cluster-name> --kubernetes-version <new-version>

Example:
```
aws eks update-cluster-version --name my-cluster --kubernetes-version 1.23
```

This will initiate the upgrade of the control plane. AWS will automatically upgrade the master node, and there will be no downtime as it will ensure high availability by upgrading control planes in a rolling manner across multiple availability zones.
You can track the progress of the upgrade:

```
aws eks describe-cluster --name <cluster-name> --query "cluster.status"
```
The status will indicate when the upgrade is complete.
4. Upgrade Worker Nodes (EC2 Instances)
After upgrading the control plane, you must upgrade the worker nodes to a compatible version. If you're using managed node groups, you can upgrade them directly.
Upgrade Managed Node Group:
```
aws eks update-nodegroup-version --cluster-name <cluster-name> --nodegroup-name <nodegroup-name> --kubernetes-version <new-version>
```
Example:
```
aws eks update-nodegroup-version --cluster-name my-cluster --nodegroup-name my-nodegroup --kubernetes-version 1.23
```

This will upgrade the worker nodes to the new Kubernetes version. EKS will automatically create new nodes, then drain and terminate the old ones. The process is non-disruptive to your workloads if you follow the correct steps for draining nodes.
# Upgrade Self-Managed Node Groups:
For self-managed node groups, you need to manually update the EC2 instance AMI, which corresponds to the new Kubernetes version. Here are the steps:
Update the EC2 Instances' AMI to use a Kubernetes-compatible AMI:
Find the latest AMI for your desired Kubernetes version:
```
aws ssm get-parameters --names "/aws/service/eks/optimized-ami/<Kubernetes-Version>/amazon-linux-2/recommended/image_id" --query "Parameters[0].Value" --output text
```

# Launch New EC2 Instances with the new AMI, ensuring that they are added to the EKS cluster and can join as worker nodes.
Drain Old Nodes (This ensures that your pods are safely moved to the new worker nodes, avoiding downtime):
```
kubectl drain <node-name> --ignore-daemonsets --delete-local-data
```

Repeat this for each old node. This will ensure that workloads are moved to the new nodes before terminating the old ones.
Terminate the Old Nodes once the new nodes are up and running.
```
aws ec2 terminate-instances --instance-ids <instance-id>
```

5. Upgrade Add-ons (CoreDNS, kube-proxy, etc.)
After upgrading the control plane and worker nodes, you also need to ensure that EKS add-ons are upgraded to the appropriate version. For example, you can update CoreDNS and kube-proxy as follows
```
aws eks update-addon --cluster-name <cluster-name> --addon-name coredns --addon-version <new-version>
aws eks update-addon --cluster-name <cluster-name> --addon-name kube-proxy --addon-version <new-version>
```

7. Verify Cluster and Workload Health
After the upgrade is completed, ensure that everything is functioning correctly:
Check Node Status:
```
kubectl get nodes
```
Ensure all nodes are in the Ready state.
Check Pod Status:
```
kubectl get pods --all-namespaces
```
Make sure that all pods are running as expected.
Verify Cluster Version:
```
kubectl version
```
This will display the client and server (control plane) versions to ensure the upgrade was successful.

Best Practices to Avoid Downtime During the Upgrade
Use Rolling Updates:
When upgrading the worker nodes, use PodDisruptionBudgets (PDBs) to ensure that a minimum number of replicas are always running.
Example of creating a PodDisruptionBudget:
yaml
```
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-pdb
spec:
  minAvailable: 1
  selector:
    matchLabels:
      app: my-app
```

Upgrade in Stages:
Upgrade the control plane first, then the worker nodes. Make sure to allow the new nodes to become fully operational before terminating the old nodes.
Monitor the Upgrade:
Use AWS CloudWatch and Kubernetes metrics to monitor the cluster’s performance and pod health during the upgrade.
Ensure Backups:
Always take regular backups of your Kubernetes configurations, including manifests, secrets, and persistent storage data.


