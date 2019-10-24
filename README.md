![alt text](https://i.imgur.com/FfIj2NA.png "Storidge Logo")

This CSI Driver interfaces a Container Orchestrator (CO), such as Kubernetes or Docker EE, to a Storidge cluster. With this driver you can dynamically provision persistent volumes from a storage abstraction layer managed by the Storidge CIO software.

The Storidge CSI driver is mostly tested on Kubernetes but should also work on other CSI based CO. Feel free to test it on other CO and give us feedback.

## Storidge CIO Overview

Storidge ContainerIO (CIO) is a fully automated, unique software stack that runs on virtual machines, cloud instances and bare metal hosts, with predictable performance. Just as Container Orchestrators create an abstraction layer on top of a group of hosts, Storidge CIO creates a storage abstraction layer for stateful applications to persist, share and manage data on logical resources.

Storidge CIO is purpose built to solve application data management challenges in new orchestrated environments. It eliminates the manual effort, inconsistencies, and tedium in trying to patch external networked storage to Container Orchestrators. Storidge CIO adds the following key capabilities to a Kubernetes cluster:

- [automatic data locality](https://docs.storidge.com/introduction/how_it_works.html#automatic-data-locality)
- [data redundancy with network RAID](https://docs.storidge.com/introduction/how_it_works.html#data-redundancy)
- [effortless high availability](https://docs.storidge.com/introduction/how_it_works.html#effortlesss-high-availability)
- [automatic capacity expansion](https://docs.storidge.com/introduction/how_it_works.html#auto-capacity-expansion)

## Kubernetes Compatibility

| CSI Driver | Kubernetes  | Description                                                           |
| -----------|:------------|:----------------------------------------------------------------------|
| 1.1.0      | 1.15+       | Supports Kubernetes 1.15+. Removes deprecated items in 1.16           |
| 1.0.0      | 1.13 - 1.15 | GA release for Kubernetes 1.13-1.15                                   |
| 0.1.0      | 1.11 - 1.12 | Beta release for Kubernetes 1.11-1.15. Requires use of feature gates  |

## Prerequisites

- Kubernetes v1.15+
- [Storidge CIO cluster](https://guide.storidge.com/getting_started/install.html)
- `--allow-privileged` must be enabled for API server and kubelet
- `--feature-gates=VolumeSnapshotDataSource=true,KubeletPluginsWatcher=true,CSINodeInfo=true,CSIDriverRegistry=true` set to true for API server and kubelet

## Deployment

### 1. Deploy Kubernetes cluster

### 2. Deploy Storidge cluster

### 3. Deploy CSI Driver

The following command deploys the CSI driver with the related volume attachment, driver registration, and provisioning sidecars:
```
kubectl create -f https://github.com/Storidge/csi-cio/blob/master/deploy/releases/csi-cio-v1.1.0.yaml
```

## Examples


## Resources

- [Documentation](https://docs.storidge.com)
- [Get Started](https://guide.storidge.com)
- [FAQ](https://faq.storidge.com)

## Support

Please join our [community Slack channel](https://storidge.com/join-cio-slack) to post questions and get support.
