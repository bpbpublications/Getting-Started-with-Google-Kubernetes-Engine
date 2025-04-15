# Commands for Overview of Google Cloud Platform

## Table of Contents

- [Commands for Overview of Google Cloud Platform](#commands-for-overview-of-google-cloud-platform)
  - [Table of Contents](#table-of-contents)
  - [Exploring GCP with Google Cloud](#exploring-gcp-with-google-cloud)
  - [GCP Networking for GKE](#gcp-networking-for-gke)
  - [GCP Storage and Database Products](#gcp-storage-and-database-products)
  - [Cloud Identity and Access Management](#cloud-identity-and-access-management)
    - [Use Case 1](#use-case-1)
    - [Use Case 2](#use-case-2)
  - [Cloud Monitoring and Logging](#cloud-monitoring-and-logging)

This document lists the commands from Chapter 2 for interacting with Google Cloud Platform (GCP) and preparing for Google Kubernetes Engine (GKE), along with brief explanations of their functionality.

## Exploring GCP with Google Cloud

- **Command**: `gcloud version`
  - **Details**: Displays the version of the `gcloud` command-line tool to verify it is installed and operational in the Cloud Shell environment.
- **Command**: `gcloud projects list`
  - **Details**: Lists all GCP projects associated with your account, showing project IDs and names.
- **Command**: `gcloud config set project YOUR_PROJECT_ID`
  - **Details**: Sets the default project for your `gcloud` session. Replace `YOUR_PROJECT_ID` with your actual project ID (e.g., `my-awesome-project`).
- **Command**: `gcloud auth login`
  - **Details**: Initiates authentication with GCP, opening a browser to log in and authorize the `gcloud` CLI.
- **Command**: `gcloud config set project PROJECT_ID`
  - **Details**: Sets a specific project as the default for `gcloud` commands. Replace `PROJECT_ID` with your project ID (e.g., `my-awesome-project`).
- **Command**: `gcloud compute instances list`
  - **Details**: Lists all Compute Engine instances in the current project, showing details like instance names and statuses.

## GCP Networking for GKE

- **Command**: `gcloud compute networks create my-gke-vpc --subnet-mode=auto`
  - **Details**: Creates a Virtual Private Cloud (VPC) network named `my-gke-vpc` with automatic subnet creation in all regions.
- **Command**: `gcloud compute networks subnets create my-gke-subnet --network=my-gke-vpc --region=us-central1 --range=192.168.0.0/24`
  - **Details**: Creates a subnet named `my-gke-subnet` within the `my-gke-vpc` VPC, located in `us-central1` with an IP range of `192.168.0.0/24`.

## GCP Storage and Database Products

- **Command**: `gsutil cp your-local-file.txt gs://your-bucket-name/`
  - **Details**: Copies a local file (`your-local-file.txt`) to a Google Cloud Storage bucket. Replace `your-bucket-name` with the name of your bucket.
- **Command**: `gcloud storage buckets create gs://my-awesome-bucket --project=my-awesome-project --location=us-central1 --storage-class=STANDARD`
  - **Details**: Creates a storage bucket named `my-awesome-bucket` in the `us-central1` region with the `STANDARD` storage class, associated with the `my-awesome-project` project.

## Cloud Identity and Access Management

### Use Case 1

- **Command**: `gcloud auth login`
  - **Details**: Authenticates the `gcloud` CLI with your GCP account by opening a browser for login (repeated from earlier section).
- **Command**: `gcloud iam service-accounts create john-doe --description="User account for John Doe" --display-name="John Doe"`
  - **Details**: Creates a service account named `john-doe` with a description and display name for user John Doe.
- **Command**: `gcloud projects add-iam-policy-binding your-project-id --member="serviceAccount:john-doe@your-project-id.iam.gserviceaccount.com" --role="roles/viewer"`
  - **Details**: Grants the `Viewer` role to the `john-doe` service account in the specified project. Replace `your-project-id` with your actual project ID.
- **Command**: `gcloud iam service-accounts keys create ~/john-doe-key.json --iam-account=john-doe@your-project-id.iam.gserviceaccount.com`
  - **Details**: Generates a private key file (`john-doe-key.json`) for the `john-doe` service account, stored in the home directory. Replace `your-project-id` with your project ID.

### Use Case 2

- **Command**: `gcloud projects create my-awesome-project`
  - **Details**: Creates a new GCP project named `my-awesome-project`.
- **Command**: `gcloud config set project my-awesome-project`
  - **Details**: Sets `my-awesome-project` as the default project for subsequent `gcloud` commands.
- **Command**: `gsutil mb -p my-awesome-project gs://my-awesome-bucket`
  - **Details**: Creates a storage bucket named `my-awesome-bucket` in the `my-awesome-project` project using the `gsutil` tool.
- **Command**: `gcloud projects add-iam-policy-binding my-awesome-project --member=user:jane.doe@example.com --role=roles/storage.objectViewer`
  - **Details**: Grants the `Storage Object Viewer` role to the user `jane.doe@example.com` in the `my-awesome-project` project.
- **Command**: `gcloud projects get-iam-policy my-awesome-project --flatten="bindings[].members" --format="table(bindings.role, bindings.members)" --filter="bindings.members:user:jane.doe@example.com"`
  - **Details**: Retrieves and displays IAM policy bindings for `my-awesome-project`, filtered to show only the roles assigned to `jane.doe@example.com` in a table format.

## Cloud Monitoring and Logging

- **Command**: `gcloud services enable monitoring.googleapis.com logging.googleapis.com`
  - **Details**: Enables the Cloud Monitoring and Logging APIs for the current project to monitor and log activities.

---
