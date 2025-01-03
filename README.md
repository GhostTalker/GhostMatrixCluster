# GhostMatrixCluster (Homelab)

This repository manages my Kubernetes cluster Powered by TrueCharts ClusterTool and using [Flux](https://fluxcd.io/). 
All applications are installed and configured via the [Helm TrueCharts](https://truecharts.org/).  

Below you will find an overview of the main components in my cluster, as well as a table of contents with brief descriptions of the included apps and services.

---

## Table of Contents

1. [Overview & Architecture](#overview--architecture)
2. [Installation & Requirements](#installation--requirements)
3. [Apps and Services](#apps-and-services)
    - [Blocky](#blocky)
    - [Cert-Manager](#cert-manager)
    - [Cloudflared](#cloudflared)
    - [CloudNative PG](#cloudnative-pg)
    - [Flux-System](#flux-system)
    - [Grafana](#grafana)
    - [HASS](#hass)
    - [Homepage](#homepage)
    - [Kubernetes Dashboard](#kubernetes-dashboard)
    - [Kyverno](#kyverno)
    - [Longhorn](#longhorn)
    - [MetalLB](#metallb)
    - [OpenEBS](#openebs)
    - [Pingvin Share](#pingvin-share)
    - [Prometheus & Alertmanager](#prometheus--alertmanager)
    - [Semaphore](#semaphore)
    - [Spegel](#spegel)
    - [Tandoor Recipes](#tandoor-recipes)
    - [Traefik](#traefik)
    - [Unpoller](#unpoller)
    - [Volsync](#volsync)
    - [System Upgrade](#system-upgrade)
    - [Other Kubernetes System Components](#other-kubernetes-system-components)
4. [Usage](#usage)
5. [Further Links](#further-links)
6. [License](#license)

---

## Overview & Architecture

This cluster runs on K3s and is deployed using Flux (GitOps methodology). All apps are managed as Helm releases via the [TrueCharts Clustertool](https://truecharts.org/). This allows every component and configuration to be versioned and automatically rolled out.

Key architecture highlights:
- **Talos** as OS for the Kubernetes nodes
- **K8s** as a Kubernetes distribution
- **Flux** as the GitOps engine
- **Renovate** as a bot for autoupdate
- **Cilium** as the Container Network Interface (CNI)
- **MetalLB** for assigning LoadBalancer IP addresses in the local network
- **Traefik** as an ingress controller
- **Longhorn** for distributed storage volumes
- **OpenEBS** for Local PV storage
- **CloudNative PG** for PostgreSQL clusters
- Various monitoring and observability tools (Prometheus, Grafana)

---

## Installation

**Use TrueCharts/Clustertool**  

   > Documentation: [TrueCharts - Getting started with ClusterTool](https://truecharts.org/clustertool/getting-started/)
1. Download ClusterTool
2. Init ClusterTool
3. Adjust Config
   - clusters/main/clusterenv.yaml
   - clusters/main/talos/talconfig.yaml
4. Generate Configuration
5. Encrypt Configuration
6. Apply Talos/Clusterconfig to the Kubernetes Nodes

---

## Apps and Services

Below is a list of the primary applications and add-ons running in this cluster. (Refer also to the outputs of `kubectl get pods -A` and `kubectl get svc -A` above for a complete overview.)

### Blocky
- **Description**: A DNS proxy and ad-blocker used to filter ads and malicious domains.
- **Key Features**: DNS caching, multiple modes for different blocklists.

### Cert-Manager
- **Description**: Automated TLS certificate management (e.g., Let’s Encrypt).
- **Key Features**: Automatic certificate issuance, renewal, and distribution.

### Cloudflared
- **Description**: Cloudflare Tunnel for secure, encrypted connections to external services.
- **Key Features**: Tunnels internal services without exposing them via a public IP.

### CloudNative PG
- **Description**: PostgreSQL operator for managing highly available Postgres databases.
- **Key Features**: Automated Postgres cluster provisioning, failover, backup.

### Flux-System
- **Description**: GitOps engine that deploys all workloads and configurations from this repository.
- **Components**: `source-controller`, `kustomize-controller`, `helm-controller`, `notification-controller`, `image-reflector-controller`, `image-automation-controller`.

### Grafana
- **Description**: Visualization and analytics platform.
- **Key Features**: Dashboards for metrics from Prometheus and other sources, alerting functionality.

### HASS
- **Description**: HomeAssistant platform.
- **Key Features**: HomeAutomation and dashboards

### Homepage
- **Description**: A personal homepage/portal to access various self-hosted services.

### Kubernetes Dashboard
- **Description**: The official Kubernetes Dashboard for cluster management via a web UI.
- **Key Features**: Workloads overview, cluster resources, namespaces, events.

### Kyverno
- **Description**: A policy engine for Kubernetes to ensure security and compliance.
- **Key Features**: Admission and mutation policies, background scans, policy reports.

### Longhorn
- **Description**: Distributed block storage for Kubernetes.
- **Key Features**: Replication, snapshots, automatic recovery in case of node failures.

### MetalLB
- **Description**: A LoadBalancer implementation for bare-metal Kubernetes clusters.
- **Key Features**: Assigns external IPs, supports ARP and BGP.

### OpenEBS
- **Description**: Container-native storage solution for dynamic volumes (Local PV).
- **Key Features**: Node-local persistent volumes, easy management through Kubernetes.

### Pingvin Share
- **Description**: Self-hosted file sharing application.
- **Key Features**: Shareable links, access management, user-friendly web interface.

### Prometheus & Alertmanager
- **Description**: A suite of monitoring and alerting tools.
- **Key Features**:
   - **Prometheus**: Metric collection and querying
   - **Alertmanager**: Handling and routing alerts (e-mail, Slack, etc.)

### Semaphore
- **Description**: Self-hosted CI/CD platform for automating builds and deployments.
- **Key Features**: Pipeline definitions, container- or VM-based jobs, Git integration.

### Spegel
- **Description**: Docker registry mirror/cache or replication tool (purpose may vary).
- **Key Features**: Speeds up image pulls, reduces Internet bandwidth usage.

### Tandoor Recipes
- **Description**: Online recipe manager.
- **Key Features**: Recipe import/export, categorization, multi-language support.

### Traefik
- **Description**: Ingress controller and reverse proxy.
- **Key Features**: Automatic service discovery, Let’s Encrypt integration, dashboard, HTTP/2, TCP/TLS support.

### Unpoller
- **Description**: Unifi Poller for Grafana dashboards and metrics collection from Unifi devices.
- **Key Features**: Fetches metrics from Unifi network devices, integrates with Prometheus.

### Volsync
- **Description**: Backup and replication solution for stateful workloads.
- **Key Features**: Secure PersistentVolume backups, replication across different sites.

### System Upgrade
- **Description**: A controller for automated upgrades of K3s clusters.
- **Key Features**: Rolling updates, scheduling, version management.

### Other Kubernetes System Components
- **Cilium**: Network and security stack (CNI) with eBPF-based filtering.
- **Node Feature Discovery**: Automatically detects hardware features and labels nodes.
- **Descheduler**: Optimizes pod distribution across the cluster.
- **snapshot-controller**: Facilitates volume snapshots.
- **Others**: `kubelet-csr-approver`, `metrics-server`, etc.

---

## Usage

1. **Make changes**  
   Edit Helm charts and Kustomize configurations in this repository (e.g., in `apps/` or `charts/`).

2. **Push to Git**  
   Once committed and pushed, Flux will detect changes and apply them to the cluster.

3. **Monitoring & Logging**  
   Monitor resources via Grafana/Prometheus or use the Kubernetes Dashboard.

4. **Scaling & Updates**  
   With GitOps (Flux), rollbacks and updates are straightforward; simply revert or update versions in Git.

---

## Further Links

- [TrueCharts Official Website](https://truecharts.org/)
- [Flux Documentation](https://fluxcd.io/docs/)
- [Kubernetes Official Documentation](https://kubernetes.io/docs/home/)
- [Renovate](https://github.com/apps/renovate)

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
