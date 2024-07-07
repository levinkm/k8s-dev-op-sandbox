

```markdown
# Kubernetes DevOps Sandbox

## Overview

This repository contains a Vagrant and Ansible setup for creating a local Kubernetes development and operations environment. It provides a single-node Kubernetes cluster with Docker, containerd, and Traefik, along with separate namespaces for development and production workloads.

## Features

- Single-node Kubernetes cluster
- Docker and containerd as container runtimes
- Traefik as an ingress controller
- Separate 'development' and 'production' namespaces
- Automated setup using Vagrant and Ansible
- Accessible from the host machine for easy management

## Prerequisites

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) (installed automatically by Vagrant)

## Quick Start

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/k8s-dev-ops-sandbox.git
   cd k8s-dev-ops-sandbox
   ```

2. Start the Vagrant VM:
   ```
   vagrant up
   ```

3. Once the VM is up and provisioned, copy the Kubernetes config to your local machine:
   ```
   cp /path/to/your/vagrant/project/kubeconfig ~/.kube/config
   ```
   Replace `/path/to/your/vagrant/project/` with the actual path to the Vagrant project directory.

4. Modify the copied config file:
   Open `~/.kube/config` and change the server address to your VM's IP:
   ```yaml
   clusters:
   - cluster:
       server: https://192.168.56.10:6443
   ```

5. Verify the setup:
   ```
   kubectl get nodes
   kubectl get namespaces
   ```

## Architecture

- **VM Specifications**: Ubuntu 20.04, 4GB RAM, 2 CPUs
- **Kubernetes Version**: 1.30.x (configurable in Ansible script)
- **Container Runtime**: containerd
- **Networking**: Flannel CNI
- **Ingress Controller**: Traefik v2.5.3

## Directory Structure

- `Vagrantfile`: Defines the VM configuration
- `k8sInstall.yml`: Ansible playbook for setting up Kubernetes and related tools
- `kubeconfig`: Generated Kubernetes config file (after provisioning)

## Customization

- Modify `Vagrantfile` to change VM settings
- Adjust `k8sInstall.yml` to add or remove components, change versions, etc.

## Usage

- **Accessing the VM**: `vagrant ssh`
- **Stopping the VM**: `vagrant halt`
- **Removing the VM**: `vagrant destroy`
- **Re-running provisioning**: `vagrant provision`

## Namespaces

Two namespaces are created by default:
- `development`: For staging and testing workloads
- `production`: For production-like workloads

Use these namespaces to separate your deployments:
```
kubectl create deployment nginx --image=nginx -n development
kubectl create deployment nginx --image=nginx -n production
```

## Troubleshooting

- If unable to connect to the cluster, ensure the API server is running and accessible:
  ```
  vagrant ssh
  sudo systemctl status kubelet
  ```
- Check Ansible logs for any provisioning errors:
  ```
  vagrant provision --debug
  ```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
