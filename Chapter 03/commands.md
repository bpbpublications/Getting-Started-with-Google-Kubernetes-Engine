# Commands for Creating and Configuring a GKE Cluster

## Table of Contents

- [Commands for Creating and Configuring a GKE Cluster](#commands-for-creating-and-configuring-a-gke-cluster)
  - [Table of Contents](#table-of-contents)
  - [Creating Your First GKE Cluster](#creating-your-first-gke-cluster)
    - [GCP Console Walkthrough for Cluster Creation](#gcp-console-walkthrough-for-cluster-creation)
    - [Create GCP Cluster via Command Line](#create-gcp-cluster-via-command-line)
    - [Create GCP Cluster using Terraform](#create-gcp-cluster-using-terraform)
      - [Terraform Code](#terraform-code)
      - [Terraform Commands](#terraform-commands)
  - [Customizing Cluster Configuration](#customizing-cluster-configuration)

This document lists the commands from Chapter 3 for creating and configuring a Google Kubernetes Engine (GKE) cluster, along with brief explanations of their functionality.

## Creating Your First GKE Cluster

### GCP Console Walkthrough for Cluster Creation

- **Command**: `gcloud services enable compute.googleapis.com`
  - **Details**: Enables the Compute Engine API for your GCP project, required for GKE cluster operations.
- **Command**: `gcloud services enable container.googleapis.com`
  - **Details**: Enables the Kubernetes Engine API for your GCP project, essential for creating and managing GKE clusters.

### Create GCP Cluster via Command Line

- **Command**: `gcloud container clusters create YOUR_CLUSTER_NAME --region YOUR_CLUSTER_REGION --num-nodes=1 --machine-type=n1-standard-2`
  - **Details**: Creates a GKE cluster with the specified name in the given region, using 1 node of type `n1-standard-2`. Replace `YOUR_CLUSTER_NAME` and `YOUR_CLUSTER_REGION` with your desired values (e.g., `us-central1`).
- **Command**: `gcloud container clusters get-credentials YOUR_CLUSTER_NAME --region YOUR_CLUSTER_REGION`
  - **Details**: Retrieves credentials for the newly created GKE cluster and configures `kubectl` to use them. Replace `YOUR_CLUSTER_NAME` and `YOUR_CLUSTER_REGION` with your actual cluster name and region.
- **Command**: `kubectl get nodes`
  - **Details**: Verifies that the nodes in your GKE cluster are ready and operational.

### Create GCP Cluster using Terraform

#### Terraform Code

Below is the Terraform configuration example provided in the text, formatted as a separate file (`main.tf`).

**`main.tf`**:

```hcl
provider "google" {
  credentials = file("<PATH_TO_YOUR_GCP_SERVICE_ACCOUNT_KEY>")
  project     = "<YOUR_GCP_PROJECT_ID>"
  region      = "us-central1"
}

resource "google_container_cluster" "my_cluster" {
  name     = "my-gke-cluster"
  location = "us-central1"

  node_pool {
    name       = "default-pool"
    machine_type = "n1-standard-2"
    disk_size_gb = 100
    disk_type    = "pd-standard"
    min_count    = 1
    max_count    = 3
  }

  master_auth {
    username = ""
    password = ""

    client_certificate_config {
      issue_client_certificate = false
    }
  }
}
```

- **Details**: This Terraform configuration defines a GKE cluster named `my-gke-cluster` in the `us-central1` region with a single node pool using `n1-standard-2` machines. Replace `<PATH_TO_YOUR_GCP_SERVICE_ACCOUNT_KEY>` with the path to your GCP service account key file and `<YOUR_GCP_PROJECT_ID>` with your GCP project ID.

#### Terraform Commands

- **Command**: `terraform init`
  - **Details**: Initializes the Terraform working directory, downloading necessary provider plugins.
- **Command**: `terraform plan`
  - **Details**: Previews the changes Terraform will make to create the GKE cluster based on `main.tf`.
- **Command**: `terraform apply`
  - **Details**: Applies the Terraform configuration to create the GKE cluster, prompting for confirmation (type `yes` to proceed).

## Customizing Cluster Configuration

- **Command**: `gcloud container clusters create [CLUSTER_NAME] --region [REGION] --enable-autopilot`
  - **Details**: Creates a GKE cluster in Autopilot mode with the specified name and region. Replace `[CLUSTER_NAME]` and `[REGION]` with your desired cluster name and region (e.g., `us-central1`).

---
