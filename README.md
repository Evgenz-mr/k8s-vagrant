# Kubernetes Vagrant Lab

A configurable local Kubernetes lab built with **Vagrant + VirtualBox + Ansible + kubeadm**.

## Goal

Create disposable Kubernetes clusters for DevOps experiments without touching unrelated VirtualBox VMs.

Default topology:

| Node | Address | vCPU | RAM |
|---|---|---:|---:|
| `ai-k8s-cp1` | `192.168.56.10` | 2 | 4 GB |
| `ai-k8s-worker1` | `192.168.56.20` | 2 | 3 GB |
| `ai-k8s-worker2` | `192.168.56.21` | 2 | 3 GB |

Each VM uses Vagrant's normal NAT adapter for Internet access plus a private/Host-Only network for Kubernetes traffic.

## Dynamic topology

Edit `config.yaml` to change the number of nodes and VM resources:

```yaml
cluster:
  control_planes: 1
  workers: 2
  network: "192.168.56"

vm:
  control_plane:
    cpus: 2
    memory: 4096
  worker:
    cpus: 2
    memory: 3072
```

For example, `workers: 4` creates `ai-k8s-worker1` through `ai-k8s-worker4` automatically.

## Components

- Vagrant — VM lifecycle and dynamic topology
- VirtualBox — virtualization provider
- Ansible — OS and Kubernetes configuration
- containerd — container runtime
- kubeadm/kubelet/kubectl — Kubernetes bootstrap and administration

## Requirements

- VirtualBox
- Vagrant
- Ansible control environment

On Windows, Ansible is best run from WSL/Linux. The Vagrant topology itself can still be controlled from Windows.

## Status

The repository currently contains the first infrastructure/Ansible skeleton. The next milestone is wiring Vagrant-generated inventory into Ansible and adding CNI, validation, and Dashboard installation before treating `vagrant up` as a one-command deployment.

## Safety

The Vagrantfile explicitly names lab VMs with the `ai-k8s-` prefix. Do not use `VBoxManage` cleanup commands against unrelated VMs.
