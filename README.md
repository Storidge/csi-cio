![alt text](https://i.imgur.com/FfIj2NA.png "Storidge Logo")

This CSI Driver interfaces a Container Orchestrator (CO), such as Kubernetes or Docker EE, to a Storidge cluster. With this driver you can dynamically provision persistent volumes from a storage abstraction layer managed by the Storidge CIO software.

The Storidge CSI driver is mostly tested on Kubernetes but should also work on other CSI based CO. Feel free to test it on other CO and give us feedback.

## Storidge CIO Overview

Storidge ContainerIO (CIO) is a fully automated, unique software stack that runs on virtual machines, cloud instances and bare metal hosts, with predictable performance. Just as Container Orchestrators create an abstraction layer on top of a group of hosts, Storidge CIO creates a storage abstraction layer for stateful applications to persist, share and manage data on logical resources.

Storidge CIO is purpose built to solve application data management challenges in new orchestrated environments. It eliminates the manual effort, inconsistencies, and tedium in trying to patch external networked storage to Container Orchestrators. Storidge CIO adds the following key capabilities to a Kubernetes cluster:

- [surpass limitations of static infrastructure](https://docs.storidge.com/introduction/how_it_works.html#surpass-infrastructure-limitations)
- [automated volume management and storage operations](https://docs.storidge.com/introduction/how_it_works.html#automated-volume-management)
- [automatic data locality](https://docs.storidge.com/introduction/how_it_works.html#automatic-data-locality)
- [data redundancy with network RAID](https://docs.storidge.com/introduction/how_it_works.html#data-redundancy)
- [effortless high availability for stateful applications](https://docs.storidge.com/introduction/how_it_works.html#effortlesss-high-availability)
- [efficient shared capacity](https://docs.storidge.com/introduction/how_it_works.html#efficient-capacity-sharing)
- [automatic capacity expansion for stateful applications](https://docs.storidge.com/introduction/how_it_works.html#auto-capacity-expansion)
- [pooled performance from storage aggregation](https://docs.storidge.com/introduction/how_it_works.html#pooled-performance)

## Kubernetes Compatibility

| CSI Driver | Kubernetes  | Description                                                     |
| -----------|:------------|:----------------------------------------------------------------|
| 1.1.0      | 1.15+       | Supports Kubernetes 1.15+. Removes deprecated items in 1.16     |
| 1.0.0      | 1.13 - 1.15 | GA release for Kubernetes 1.13-1.15                             |
| 0.1.0      | 1.11 - 1.12 | Beta release for Kubernetes 1.11-1.15. Requires feature gates   |

## Prerequisites

***This CSI Driver will not function without a copy of the Storidge ContainerIO software installed on your cluster.***
*The free Community Edition and the Enterprise Editions are [available through our website](https://guide.storidge.com). For support, please contact us through [our Slack channel.](https://storidge.com/join-cio-slack)*

## Installation and Configuration

This plugin requires an installation of CIO to work. Once you have a working CIO cluster, the CSI Driver can be deployed. [To install ContainerIO for free, please visit our website.](https://storidge.com/cio-portainer/)

It is recommended to always use the latest available version, which requires Kubernetes 1.13+. However, legacy versions are available.

The latest version is `v1.1.0`

Simply run:

```
kubectl create -f https://github.com/Storidge/csi-cio/blob/master/deploy/releases/csi-cio-v1.1.0.yaml
```

This will start the CSI Driver on your cluster.

## Resources
- [Documentation](https://docs.storidge.com)
- [Get Started](https://guide.storidge.com)
- [FAQ](https://faq.storidge.com)

## Support

Refer to FAQs at https://faq.storidge.com/. Enter in keywords in the search bar to get quick answers.

Also please join our [community Slack channel]((https://storidge.com/join-cio-slack)) to post questions and get support.
