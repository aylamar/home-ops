<div align="center">

<img src="https://raw.githubusercontent.com/joryirving/home-ops/main/docs/src/assets/icons/kah-logo.png" align="center" width="144px" height="144px"/>


### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f680/512.gif" alt="🚀" width="16" height="16"> My Home Operations Repository <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f6a7/512.gif" alt="🚧" width="16" height="16">

_... managed with Flux, Renovate, and GitHub Actions_ <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f916/512.gif" alt="🤖" width="16" height="16">

</div>

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f4a1/512.gif" alt="💡" width="20" height="20"> Overview

This is a repo containing that I use to manage my home kubernetes cluster. My cluster currently runs on [Talos](https://www.talos.dev/) which provides a secure and immutable environment for Kubernetes. I try to adhere to Infrastructure as Code (IaC) and GitOps practices using the tools like [Ansible](https://www.ansible.com/), [Kubernetes](https://kubernetes.io/), [Flux](https://github.com/fluxcd/flux2), [Renovate](https://github.com/renovatebot/renovate) and [GitHub Actions](https://github.com/features/actions).

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f331/512.gif" alt="🌱" width="20" height="20"> Kubernetes

My Kubernetes cluster is deployed with [Talos](https://www.talos.dev). This is a semi-hyper-converged cluster, workloads and block storage are sharing the same available resources on my nodes while I have a separate server with ZFS for NFS/SMB shares, bulk file storage and backups.

This repo is cluster is originally based off of [onedr0p/cluster-template](https://github.com/onedr0p/cluster-template) if you want to follow along with some of the practices I use here.

### Core Components

- [cert-manager](https://github.com/cert-manager/cert-manager): Creates SSL certificates for services in my cluster.
- [cilium](https://github.com/cilium/cilium): Internal Kubernetes container networking interface.
- [cloudflared](https://github.com/cloudflare/cloudflared): Enables Cloudflare secure access to certain ingresses.
- [external-dns](https://github.com/kubernetes-sigs/external-dns): Automatically syncs ingress DNS records to a DNS provider.
- [external-secrets](https://github.com/external-secrets/external-secrets): Managed Kubernetes secrets using [1Password Connect](https://github.com/1Password/connect).
- [ingress-nginx](https://github.com/kubernetes/ingress-nginx): Kubernetes ingress controller using NGINX as a reverse proxy and load balancer.
- [rook](https://github.com/rook/rook): Distributed block storage for peristent storage.
- [sops](https://github.com/getsops/sops): Managed secrets for Kubernetes and Terraform which are commited to Git.
- [spegel](https://github.com/spegel-org/spegel): Stateless cluster local OCI registry mirror.
- [volsync](https://github.com/backube/volsync): Backup and automated recovery of persistent volume claims.

### GitOps

[Flux](https://github.com/fluxcd/flux2) watches the clusters in my [kubernetes](./kubernetes/) folder (see Directories below) and makes the changes to my clusters based on the state of my Git repository.

Flux will recursively search the `kubernetes/apps` folder until it finds the most top level `kustomization.yaml` per directory and then apply all the resources listed in it. That aforementioned `kustomization.yaml` will generally only have a namespace resource and one or many Flux kustomizations (`ks.yaml`). Under the control of those Flux kustomizations there will be a `HelmRelease` or other resources related to the application which will be applied.

[Renovate](https://github.com/renovatebot/renovate) watches my **entire** repository looking for dependency updates, when they are found a PR is automatically created. When some PRs are merged Flux applies the changes to my cluster.

### Directories
This Git repository contains the following directories under kubernetes.

```sh
📁 kubernetes      # Kubernetes cluster defined as code
├─📁 apps          # Apps deployed into my cluster grouped by namespace
├─📁 bootstrap     # Flux installation
└─📁 flux          # Main Flux configuration of repository
└─📁 templates     # Templates used by multiple apps
```

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/2699_fe0f/512.gif" alt="⚙" width="20" height="20"> Hardware

| Device                                  | OS Disk Size | Data Disk Size            | RAM  | Purpose                                          |
|-----------------------------------------|--------------|---------------------------|------|--------------------------------------------------|
| 3x Intel NUC 13 NUC13ANHi5              | 512GB SSD    | 1TB NVMe                  | 32GB | Talos node, NVMe is dedicated to Rook Ceph       |
| HL15                                    | 1TB NVMe     | 10x 20TB (Mirrored vdevs) | 64GB | TrueNAS powered bulk storage                     |
| Unraid                                  | 32GB USB     | 8x 8TB HDDs (1 Parity)    | 32GB | Legacy bulk storage                              |
| UniFi UDM Pro                           | -            | -                         | -    | Firewall, router, and NVR                        |
| Switch Pro Max 16 PoE                   | -            | -                         | -    | Main switch                                      |
| Switch Flex Mini 2.5G                   | -            | -                         | -    | Switch for NUCs                                  |
| UniFi USP PDU Pro                       | -            | -                         | -    | PDU                                              |
| CyberPower CP1500AVRLCD3                | -            | -                         | -    | UPS                                              |


## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f64f/512.gif" alt="🙏" width="20" height="20"> Thanks

Thanks to all the people who donate their time to the [Home Operations](https://discord.gg/home-operations) community. The major inspirations for this cluster were [onedr0p](https://github.com/onedr0p/home-ops), [bjw-s](https://github.com/bjw-s-labs/home-ops), [axeII](https://github.com/axeII/home-ops/), and [joryirving](https://github.com/joryirving/home-ops).

Be sure to check out [kubesearch.dev](https://kubesearch.dev/) for ideas on how to deploy applications or get ideas on what you could deploy.
