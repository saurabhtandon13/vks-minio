# MinIO on Kubernetes

This project provides resources for deploying MinIO, a high-performance object storage solution, on a Kubernetes cluster using Helm charts.

## Project Structure

This repository contains Helm charts for deploying:

*   **MinIO Operator:** Manages MinIO tenants within the Kubernetes cluster.
*   **MinIO Tenant:** Represents a MinIO object storage cluster.

The repository is organized as follows:

*   `minio-k8s/`: Contains Kubernetes manifests and Kustomization files for a specific MinIO setup. (More details might be needed here if this is a primary way of deployment)
*   `minio-operator-deployment/`: Contains the Helm chart for deploying the MinIO Operator.
*   `minio-tenant-deployment/`: Contains the Helm chart for deploying a MinIO Tenant.

## Prerequisites

Before you begin, ensure you have the following:

*   A running Kubernetes cluster.
*   [Helm](https://helm.sh/docs/intro/install/) installed.
*   `kubectl` configured to interact with your Kubernetes cluster.

## Deployment Steps

Follow these steps to deploy MinIO on your Kubernetes cluster:

### 1. Add MinIO Helm Repository

Add the official MinIO Helm repository to your Helm client:

```bash
helm repo add minio https://operator.min.io/
helm repo update
```

### 2. Install MinIO Operator

The MinIO Operator is responsible for managing MinIO tenants. Install it using the provided Helm chart:

```bash
helm install \\
  --namespace minio-operator \\
  --create-namespace \\
  minio-operator minio/operator
```

You can find the chart and its default values in the `minio-operator-deployment/operator/` directory.

### 3. Install MinIO Tenant

Once the operator is running, you can deploy a MinIO Tenant. A tenant represents your MinIO object storage cluster.

```bash
helm install --namespace tenant-ns \\
  --create-namespace tenant minio/tenant
```

This command deploys a MinIO Tenant in the `tenant-ns` namespace. By default, this creates a 4-node MinIO cluster.

You can find the chart and its default values in the `minio-tenant-deployment/tenant/` directory.

## Configuration

Both the MinIO Operator and MinIO Tenant Helm charts can be customized using their respective `values.yaml` files.

*   **Operator Configuration:** Modify `minio-operator-deployment/operator/values.yaml` to customize the operator deployment.
*   **Tenant Configuration:** Modify `minio-tenant-deployment/tenant/values.yaml` to customize aspects of your MinIO tenant, such as the number of nodes, storage capacity, and other MinIO settings. You can also refer to the official MinIO tenant values documentation [here](https://github.com/minio/operator/blob/master/helm/tenant/values.yaml).

The `minio-k8s/` directory also contains Kustomization files and Kubernetes manifests that might be used for a more customized or GitOps-based deployment approach. Refer to the `minio-k8s/kustomization.yaml` and the files within `minio-k8s/resources/` for more details if you choose this deployment method.

## License

This project, including the MinIO software and associated Helm charts, is licensed under the GNU AGPLv3 or later. See the [LICENSE](LICENSE) file for more details.

MinIO is a product of MinIO, Inc. For more detailed documentation on MinIO, please visit the [official MinIO documentation](https://docs.min.io/).
