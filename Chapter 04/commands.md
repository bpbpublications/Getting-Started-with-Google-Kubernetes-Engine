

# Commands for Setting Up a GKE Environment

## Table of Contents
- [Commands for Setting Up a GKE Environment](#commands-for-setting-up-a-gke-environment)
  - [Table of Contents](#table-of-contents)
  - [Preparing Your System](#preparing-your-system)
    - [Installing Visual Studio Code (VS Code)](#installing-visual-studio-code-vs-code)
      - [For MacBook](#for-macbook)
      - [For Windows](#for-windows)
    - [Installing GCloud SDK and kubectl](#installing-gcloud-sdk-and-kubectl)
      - [For MacBook](#for-macbook-1)
      - [For Windows](#for-windows-1)
    - [Using Homebrew and Chocolatey](#using-homebrew-and-chocolatey)
    - [Verifying Installation](#verifying-installation)
    - [Authenticating Local System with GKE Cluster](#authenticating-local-system-with-gke-cluster)
  - [GKE Management and Automation Tools](#gke-management-and-automation-tools)
    - [Kustomize](#kustomize)
      - [For Mac and Linux](#for-mac-and-linux)
      - [For Windows](#for-windows-2)
    - [Helm](#helm)
      - [For MacBook and Linux](#for-macbook-and-linux)
      - [For Windows](#for-windows-3)
    - [ArgoCD](#argocd)
      - [In GKE Cluster](#in-gke-cluster)
      - [For MacBook and Linux](#for-macbook-and-linux-1)
      - [For Windows](#for-windows-4)
    - [Prometheus and Grafana](#prometheus-and-grafana)
      - [In GKE Cluster](#in-gke-cluster-1)
      - [Terraform/Service Manifest (Before and After)](#terraformservice-manifest-before-and-after)
    - [OpenLens](#openlens)
      - [For Windows](#for-windows-5)
      - [For MacBook and Linux](#for-macbook-and-linux-2)
    - [K9s](#k9s)
      - [For Windows](#for-windows-6)
      - [For MacBook and Linux](#for-macbook-and-linux-3)

This document lists the commands from Chapter 4 for setting up and managing a Google Kubernetes Engine (GKE) environment, along with brief explanations of their functionality.

## Preparing Your System

### Installing Visual Studio Code (VS Code)

#### For MacBook
- **Command**: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
  - **Details**: Installs Homebrew, a package manager for macOS, if not already present. Used as a prerequisite for installing other tools.
- **Command**: `brew install --cask visual-studio-code`
  - **Details**: Installs Visual Studio Code using Homebrew, a popular code editor for Kubernetes-related tasks.

#### For Windows
- **Command**: `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`
  - **Details**: Installs Chocolatey, a package manager for Windows, by bypassing execution policy and downloading the installation script.
- **Command**: `choco install vscode`
  - **Details**: Installs Visual Studio Code using Chocolatey.

### Installing GCloud SDK and kubectl

#### For MacBook
- **Command**: `curl https://sdk.cloud.google.com | bash`
  - **Details**: Downloads and runs the Google Cloud SDK installation script to install the `gcloud` CLI.
- **Command**: `exec -l $SHELL`
  - **Details**: Restarts the shell to apply changes after installing the Google Cloud SDK.
- **Command**: `gcloud components install kubectl`
  - **Details**: Installs `kubectl`, the Kubernetes command-line tool, as a component of the Google Cloud SDK.

#### For Windows
- **Command**: `(New-Object System.Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe"); Start-Process -FilePath "$env:Temp\GoogleCloudSDKInstaller.exe"`
  - **Details**: Downloads the Google Cloud SDK installer executable and launches it to install the `gcloud` CLI.
- **Command**: `gcloud components install kubectl`
  - **Details**: Installs `kubectl` as a component of the Google Cloud SDK after the SDK is installed.

### Using Homebrew and Chocolatey

- **For MacBook**:
  - **Command**: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
    - **Details**: Installs Homebrew on macOS (repeated from VS Code installation).
  - **Command**: `brew install --cask google-cloud-sdk && gcloud components install kubectl`
    - **Details**: Installs the Google Cloud SDK using Homebrew and then installs `kubectl` as a component.
- **For Windows**:
  - **Command**: `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`
    - **Details**: Installs Chocolatey on Windows (repeated from VS Code installation).
  - **Command**: `choco install gcloudsdk && gcloud components install kubectl`
    - **Details**: Installs the Google Cloud SDK using Chocolatey and then installs `kubectl`.

### Verifying Installation

- **Command**: `gcloud version`
  - **Details**: Displays the installed version of the Google Cloud SDK to verify it is correctly installed.
- **Command**: `kubectl version`
  - **Details**: Displays the installed version of `kubectl` to confirm it is functional and connected to a cluster (if configured).

### Authenticating Local System with GKE Cluster

- **Command**: `gcloud auth login`
  - **Details**: Initiates authentication with Google Cloud, opening a browser to log in and authorize the CLI.
- **Command**: `gcloud auth application-default login`
  - **Details**: Sets up default credentials for local application development with GCP services, opening a browser for login.
- **Command**: `gcloud config set project [PROJECT_ID]`
  - **Details**: Configures the `gcloud` CLI to use a specific GCP project by its project ID (replace `[PROJECT_ID]` with your actual ID).
- **Command**: `gcloud services enable compute.googleapis.com`
  - **Details**: Enables the Compute Engine API for the specified project.
- **Command**: `gcloud services enable container.googleapis.com`
  - **Details**: Enables the Kubernetes Engine API for the specified project.
- **Command**: `gcloud container clusters create [CLUSTER_NAME] --region [REGION]`
  - **Details**: Creates a new GKE cluster with the specified name and region (replace `[CLUSTER_NAME]` and `[REGION]`).
- **Command**: `gcloud container clusters create gke-cluster --region "us-west1"`
  - **Details**: Creates a GKE cluster named `gke-cluster` in the `us-west1` region.
- **Command**: `gcloud container clusters get-credentials gke-cluster --region us-west1 --project [PROJECT_ID]`
  - **Details**: Retrieves credentials for the `gke-cluster` in `us-west1` and configures `kubectl` to use them (replace `[PROJECT_ID]`).

## GKE Management and Automation Tools

### Kustomize

#### For Mac and Linux
- **Command**: `curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash`
  - **Details**: Downloads and executes a script to install the `kustomize` CLI locally.
- **Command**: `find . -name kustomize`
  - **Details**: Searches the current directory for the `kustomize` binary to locate it after installation.
- **Command**: `mv kustomize /usr/local/bin/`
  - **Details**: Moves the `kustomize` binary to `/usr/local/bin/` to make it accessible in the PATH.
- **Command**: `chmod +x /usr/local/bin/kustomize`
  - **Details**: Makes the `kustomize` binary executable.
- **Command**: `brew install kustomize`
  - **Details**: Installs `kustomize` using Homebrew as an alternative method.
- **Command**: `kustomize version`
  - **Details**: Verifies the installed version of `kustomize`.

#### For Windows
- **Command**: `choco install kustomize`
  - **Details**: Installs `kustomize` using Chocolatey.
- **Command**: `scoop install kustomize`
  - **Details**: Installs `kustomize` using Scoop, an alternative package manager.
- **Command**: `kustomize version`
  - **Details**: Verifies the installed version of `kustomize`.

### Helm

#### For MacBook and Linux
- **Command**: `curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3`
  - **Details**: Downloads the Helm installation script to a file named `get_helm.sh`.
- **Command**: `chmod 700 get_helm.sh`
  - **Details**: Sets execute permissions for the `get_helm.sh` script.
- **Command**: `./get_helm.sh`
  - **Details**: Runs the Helm installation script to install the `helm` CLI.
- **Command**: `brew install helm`
  - **Details**: Installs `helm` using Homebrew as an alternative method.
- **Command**: `sudo snap install helm --classic`
  - **Details**: Installs `helm` using Snap with classic confinement.

#### For Windows
- **Command**: `choco install helm`
  - **Details**: Installs `helm` using Chocolatey.
- **Command**: `scoop install helm`
  - **Details**: Installs `helm` using Scoop.

### ArgoCD

#### In GKE Cluster
- **Command**: `kubectl create ns argocd`
  - **Details**: Creates a namespace named `argocd` in the Kubernetes cluster.
- **Command**: `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
  - **Details**: Applies the ArgoCD installation manifest to the `argocd` namespace.

#### For MacBook and Linux
- **Command**: `brew install argocd`
  - **Details**: Installs the ArgoCD CLI using Homebrew.

#### For Windows
- **Command**: `choco install argocd-cli`
  - **Details**: Installs the ArgoCD CLI using Chocolatey.
- **Command**: `scoop install argocd`
  - **Details**: Installs the ArgoCD CLI using Scoop.

### Prometheus and Grafana

#### In GKE Cluster
- **Command**: `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
  - **Details**: Adds the Prometheus Community Helm chart repository.
- **Command**: `helm repo update`
  - **Details**: Updates the local Helm repository list to ensure the latest chart versions.
- **Command**: `kubectl create namespace prometheus`
  - **Details**: Creates a namespace named `prometheus` in the Kubernetes cluster.
- **Command**: `helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus`
  - **Details**: Installs the Prometheus stack in the `prometheus` namespace using Helm.
- **Command**: `kubectl get all -n prometheus`
  - **Details**: Lists all resources (pods, services, etc.) in the `prometheus` namespace.
- **Command**: `kubectl get pods -n prometheus | grep prometheus-prometheus-kube-prometheus-prometheus-0`
  - **Details**: Filters and checks for the specific Prometheus pod.
- **Command**: `kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090 -n prometheus`
  - **Details**: Forwards port 9090 from the local machine to the Prometheus pod for UI access.
- **Command**: `kubectl get svc -n prometheus`
  - **Details**: Lists all services in the `prometheus` namespace to get Grafana service details.
- **Command**: `kubectl edit service prometheus-grafana -n prometheus`
  - **Details**: Opens the `prometheus-grafana` service manifest in an editor to modify its type (e.g., to `LoadBalancer`).
- **Command**: `kubectl get service -n prometheus --watch`
  - **Details**: Watches the services in the `prometheus` namespace in real-time to observe updates (e.g., LoadBalancer IP).
- **Command**: `kubectl get secret prometheus-grafana -o jsonpath="{.data.admin-password}" -n prometheus | base64 --decode ; echo`
  - **Details**: Retrieves and decodes the Grafana admin password from a Kubernetes secret.

#### Terraform/Service Manifest (Before and After)
Below is the service manifest example provided in the text, formatted as YAML. This is not a command but a configuration file edited via `kubectl edit service`.

**Before (ClusterIP)**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-grafana
  namespace: prometheus
spec:
  type: ClusterIP
  ports:
    - port: 3000
      targetPort: 3000
  selector:
    app: Grafana
```

**After (LoadBalancer)**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-grafana
  namespace: prometheus
spec:
  type: LoadBalancer
  ports:
    - port: 3000
      targetPort: 3000
  selector:
    app: Grafana
```

### OpenLens

#### For Windows
- **Command**: `choco install openlens --version=6.1.12`
  - **Details**: Installs OpenLens version 6.1.12 using Chocolatey.

#### For MacBook and Linux
- **Command**: `brew install --cask openlens`
  - **Details**: Installs OpenLens using Homebrew.

### K9s

#### For Windows
- **Command**: `choco install k9s`
  - **Details**: Installs K9s using Chocolatey.

#### For MacBook and Linux
- **Command**: `brew install k9s`
  - **Details**: Installs K9s using Homebrew.

