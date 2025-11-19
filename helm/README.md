# Helm Cheatsheet

## Overview
Helm is the package manager for Kubernetes that helps you define, install, and upgrade complex Kubernetes applications using charts.

## Install Helm

```bash
# Install via script (Linux/macOS)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Install via package managers (macOS)
brew install helm

# Verify installation
helm version
```

## Repository Management

```bash
# Add a repository
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/

# Update repositories
helm repo update

# List configured repositories
helm repo list
helm repo list -o yaml

# Remove a repository
helm repo remove <repo-name>

# Search for charts in repositories
helm search repo traefik
helm search repo traefik --versions           # show all versions

# Search Helm Hub
helm search hub metrics-server

# Show chart information
helm show chart metrics-server/metrics-server
helm show values metrics-server/metrics-server
helm show readme metrics-server/metrics-server
helm show all metrics-server/metrics-server
```

## Install Charts

```bash
# Basic installation (default namespace)
helm install metrics-server metrics-server/metrics-server

# Install in specific namespace
helm install metrics-server metrics-server/metrics-server --namespace kube-system

# Install and create the namespace
helm install traefik traefik/traefik -n traefik --create-namespace

# Install with inline values
helm install traefik traefik/traefik -n traefik --create-namespace --set service.type=LoadBalancer

# Install with values file
helm install traefik traefik/traefik -f values.yaml

# Install from local chart
helm install traefik ./traefik

# Install from packaged chart
helm install traefik ./traefik-37.3.0.tgz

# Install with specific version
helm install metrics-server metrics-server/metrics-server --version 3.12.2

# Wait for deployment to complete
helm install metrics-server metrics-server/metrics-server --wait --timeout 5m

# Dry run & debug installation
helm install traefik traefik/traefik --dry-run
helm install traefik traefik/traefik --dry-run --debug

# Fetch and extract a chart
helm pull traefik/traefik
helm pull traefik/traefik --untar

# Validate chart syntax
helm lint traefik
```

## Create & Develop Charts

```bash
# Create a new chart
helm create my-chart

# Package chart into tarball
helm package my-chart
```

## Release Management

```bash
# List releases in specific namespace
helm list -n argo

# List all releases across all namespaces
helm list -A                     # default --output table
helm list -A --output yaml
helm list -A --output json

# Get specific field
helm list -A --output json | jq '.[] | .name'

# Show release details
helm status argo -n argo

# Get user-supplied values
helm get values argo -n argo

# Get all of the values used by a release (including the defaults)
helm get values argo -n argo --all

# Get release manifest (rendered YAML)
helm get manifest argo -n argo

# Get release notes
helm get notes argo -n argo

# Get all the information about a named release
helm get all argo -n argo

# Show release history
helm history argo -n argo
helm history argo -n argo --output json
```

