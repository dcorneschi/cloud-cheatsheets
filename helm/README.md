# Helm Cheatsheet

## Overview
Helm is the package manager for Kubernetes that helps you define, install, and upgrade complex Kubernetes applications using charts.

## Install Helm

```bash
# Install via script (Linux/macOS)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Install via package managers (macOS)
brew install helm
```

## Repository Management

```bash
# Add a repository
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/

# Update repositories
helm repo update

# List configured repositories
helm repo list

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

# Install in a new namespace
helm install traefik traefik/traefik -n traefik --create-namespace

# Install with custom values
helm install traefik traefik/traefik -n traefik --create-namespace --set service.type=LoadBalancer

# Install with values file
helm install traefik traefik/traefik -f values.yaml

# Install from local chart
helm install traefik ./traefik

# Install with specific version
helm install metrics-server metrics-server/metrics-server --version 3.12.2

# Dry run & debug installation
helm install traefik traefik/traefik --dry-run
helm install traefik traefik/traefik --dry-run --debug
```