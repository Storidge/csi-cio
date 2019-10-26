## Storidge CSI Driver Overview

![alt text](https://i.imgur.com/FfIj2NA.png "Storidge Logo")

This project contains the resources required to get Storidge Persistent Volumes working in conjunction with your Kubernetes cluster.

- [Getting Started with Kubernetes]()
- [Storidge Documentation](https://docs.storidge.com)

## Overview

Storidge Inc's ContainerIO Software simplifies DevOps by adding a persistent storage layer to your cluster of virtual machines, physical servers, and/or cloud instances. By creating a distributed storage layer, ContainerIO adds the following capabilities to any deployment:

- [surpass limitations of static infrastructure](https://docs.storidge.com/introduction/how_it_works.html#surpass-infrastructure-limitations)
- [automated volume management and storage operations](https://docs.storidge.com/introduction/how_it_works.html#automated-volume-management)
- [automatic data locality](https://docs.storidge.com/introduction/how_it_works.html#automatic-data-locality)
- [data redundancy with network RAID](https://docs.storidge.com/introduction/how_it_works.html#data-redundancy)
- [effortless high availability for stateful applications](https://docs.storidge.com/introduction/how_it_works.html#effortlesss-high-availability)
- [efficient shared capacity](https://docs.storidge.com/introduction/how_it_works.html#efficient-capacity-sharing)
- [automatic capacity expansion for stateful applications](https://docs.storidge.com/introduction/how_it_works.html#auto-capacity-expansion)
- [pooled performance from storage aggregation](https://docs.storidge.com/introduction/how_it_works.html#pooled-performance)

This CSI Driver integrates the ContainerIO Volumes with any Container Orchestrator that supports the [Container Storage Interface specification](https://github.com/container-storage-interface/spec). With this driver installed, users can enjoy decades of storage expertise with minimal effort required. Container Orchestrators can leverage Storidge Volumes to:

- recover from node failure, with data intact, with no operator intervention
- provision, backup, and expand volumes automatically
- move stateful jobs across nodes with no service downtime
- scale clusters up and down to match demand
- more

Additionally, ContainerIO comes packaged with the popular, powerful, and easy-to-use [Portainer.io](https://www.portainer.io/) UI.

Learn More:

- [Storidge Website](https://storidge.com)
- [Documentation](https://docs.storidge.com)
- [Install CIO Now](https://guide.storidge.com)
- [Join the CIO Slack Channel](https://storidge.com/join-cio-slack)
- [View the Storidge GitHub](https://github.com/storidge)

## Prerequisites

***This CSI Driver will not function without a copy of the Storidge ContainerIO software installed on your cluster.***
*The free Community Edition and the Enterprise Editions are [available through our website](https://guide.storidge.com). For support, please contact us through [our Slack channel.](https://storidge.com/join-cio-slack)*


There are a number of Prerequisites that must be fulfilled before CIO can be successfully installed.

### Hardware

Storidge ContainerIO has the following hardware requirements. The requirements differ based on where you are trying to run CIO.

While ContainerIO can run in a single node configuration, it is not recommended. Multi-node operation **requires a minimum of three nodes**, and four+ nodes are recommended for production environments. Additionally, a **minimum of 3 or more dedicated data drives per node are required** to ensure data redundancy.

|   | Bare Metal | Virtual Server/Cloud Instance | Notes |
| --- | --- | --- | --- |
| CPU | Dual-Core Intel x86 or better | 2+ Virtual CPUs |   |
| RAM | 64GB | 16GB |   |
| Data Drive | 20GB | 20GB |   |
| Boot Drive | 3+ 100GB SSDs | 3+ 100GB SSDs | Community edition allows up to 1TB per node. **3+ drives are required per node** as a means to ensure data redundancy. |
| Ethernet NIC | Dual 10GigE | Dual 1GigE+ | Storidge CIO will operate off of one network interface; dual interfaces provide significantly better performance. One should be allotted to internode storage traffic, and the other to front end applications. |

### Software

Storidge CIO can be installed if the following requirements are met.

| Software | Release | Kernel |
| --- | --- | --- |
| CentOS | 7.6+ | 3.10+ |
| Ubuntu | 16.04 | 4.4.0+ |
| Ubuntu | 18.04 | 4.15.0+ |
| Kubernetes | 1.11+ |   |

Currently, CentOS 8 support is in development. If a new kernel version is unsupported by CIO, [please contact us through our Storidge Slack Channel](https://storidge.com/join-cio-slack) so that we can build for the new kernel.

## Installation and Configuration

This plugin requires an installation of CIO to work. Once you have a working CIO cluster, the CSI Driver can be deployed. [To install ContainerIO for free, please visit our website.](https://storidge.com/cio-portainer/)

It is recommended to always use the latest available version, which requires Kubernetes 1.13+. However, legacy versions are available.

The latest version is `v1.1.0`

Simply run:

```
kubectl create -f https://raw.githubusercontent.com/Storidge/csi-cio/master/deploy/releases/csi-cio-v1.1.0.yaml
```

This will start the CSI Driver on your cluster.

## Revision History

| Revision | K8s Version | Notes |
| --- |  --- | --- |
| 1.1.0 | 1.15+ | Support for Kubernetes 1.15+ and removal of deprecated items introduced in 1.16 |
| 1.0.0 | 1.13 - 1.15 | Beta release of CSI Driver for Kubernetes 1.13-1.15 |
| 0.1.0 | 1.11 - 1.12 | Alpha release for Kubernetes 1.11-1.15. Requires the use of Feature Gates |

## Further Reading

Refer to the following links for additional information:

- [Storidge Website](https://storidge.com)
- [Documentation](https://docs.storidge.com)
- [Install CIO Now](https://guide.storidge.com)
- [Join the Storidge Slack Channel](https://storidge.com/join-cio-slack)
- [View the Storidge GitHub](https://github.com/storidge)
