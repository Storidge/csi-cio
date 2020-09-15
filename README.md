![alt text](https://i.imgur.com/FfIj2NA.png "Storidge Logo")

This CSI Driver interfaces a Container Orchestrator (CO), such as Kubernetes or Docker EE, to a Storidge cluster. With this driver you can dynamically provision persistent volumes from a storage abstraction layer managed by the Storidge CIO software.

The [Storidge CSI driver](https://hub.docker.com/_/storidge-csi-driver) is mostly tested on Kubernetes but should also work on other CSI based CO. Feel free to test it on other CO and give us feedback.

## Storidge CIO Overview

[Storidge ContainerIO](https://guide.storidge.com/what_is_cio/introduction.html#why-cio) (CIO) is a fully automated, unique software stack that runs on virtual machines, cloud instances and bare metal hosts, with predictable performance. Just as Container Orchestrators create an abstraction layer on top of a group of hosts, Storidge CIO creates a storage abstraction layer for stateful applications to persist, share and manage data on logical resources.

[Storidge CIO](https://docs.storidge.com/introduction/how_it_works.html) is purpose built to solve application data management challenges in new orchestrated environments. It eliminates the manual effort, inconsistencies, and tedium in trying to patch external networked storage to Container Orchestrators. Storidge CIO adds the following key capabilities to a Kubernetes cluster:

- [automatic data locality](https://docs.storidge.com/introduction/how_it_works.html#automatic-data-locality)
- [data redundancy with network RAID](https://docs.storidge.com/introduction/how_it_works.html#data-redundancy)
- [effortless high availability](https://docs.storidge.com/introduction/how_it_works.html#effortlesss-high-availability)
- [automatic capacity expansion](https://docs.storidge.com/introduction/how_it_works.html#auto-capacity-expansion)

## Kubernetes Compatibility

| CSI Driver | Kubernetes  | Description                                                           |
| -----------|:------------|:----------------------------------------------------------------------|
| 1.6.0      | 1.16+       | Failover support and stability improvements                           |
| 1.5.0      | 1.16+       | Improved snapshot support and bug fixes                               |
| 1.4.0      | 1.16+       | Adds dependency to attacher v2.1.1 and fixed RBAC issue               |
| 1.3.0      | 1.16+       | Supports Kubernetes 1.16+                                             |
| 1.1.0      | 1.15-       | Supports Kubernetes 1.15-. Removes deprecated items in 1.16           |
| 1.0.0      | 1.13 - 1.15 | GA release for Kubernetes 1.13-1.15                                   |

## Prerequisites

- Kubernetes v1.15+
- Storidge CIO cluster
- `--allow-privileged` must be enabled for API server and kubelet
- (Optional for Kubernetes v1.17+ if snapshot support is needed) `--feature-gates=VolumeSnapshotDataSource=true` set to true for API server and kubelet

## Deployment Sequence

### 1. Deploy Kubernetes cluster

### 2. [Deploy Storidge cluster](https://guide.storidge.com/getting_started/install.html)

### 3. Deploy CSI Driver

The following command deploys the Storidge CSI driver with related volume attachment, driver registration, and provisioning sidecars:

**Kubernetes 1.16**
```
kubectl create -f https://raw.githubusercontent.com/Storidge/csi-cio/master/deploy/releases/csi-cio-v1.6.0.yaml
```

**Kubernetes 1.15 and below**
```
kubectl create -f https://raw.githubusercontent.com/Storidge/csi-cio/master/deploy/releases/csi-cio-v1.1.0.yaml
```

## Examples - Basic

### Create [storage class](https://docs.storidge.com/kubernetes_storage/storage_classes.html) with NGINX [profile](https://docs.storidge.com/cio_cli/profile.html#cio-profile-create)

```
# cat cio-sc-nginx.yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: cio-nginx
provisioner: csi.cio.storidge.com
parameters:
  profile: NGINX
reclaimPolicy: Delete
allowVolumeExpansion: true

# kubectl create -f cio-sc-nginx.yaml
storageclass.storage.k8s.io/cio-nginx created
```

Verify storage class was created
```
# kubectl get sc
NAME                    PROVISIONER            AGE
cio-default (default)   csi.cio.storidge.com   56m
cio-nginx               csi.cio.storidge.com   5s
```

### Create persistent volume claim (RWO access)

```
# cat nginx-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  accessModes:
  - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 1Gi
  storageClassName: cio-nginx

# kubectl create -f nginx-pvc.yaml
persistentvolumeclaim/nginx-pvc created
```

Confirm pvc creation
```
# kubectl get pvc
NAME        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
nginx-pvc   Bound    pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb   1Gi        RWO            cio-nginx      1m46s

# kubectl describe pvc
Name:          nginx-pvc
Namespace:     default
StorageClass:  cio-nginx
Status:        Bound
Volume:        pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb
Labels:        <none>
Annotations:   kubectl.kubernetes.io/last-applied-configuration:
                 {"apiVersion":"v1","kind":"PersistentVolumeClaim","metadata":{"annotations":{},"name":"nginx-pvc","namespace":"default"},"spec":{"accessMo...
               pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
               volume.beta.kubernetes.io/storage-provisioner: csi.cio.storidge.com
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Mounted By:    <none>
Events:
  Type     Reason                 Age    From                                                                Message
  ----     ------                 ----   ----                                                                -------
  Normal   ExternalProvisioning   2m57s  persistentvolume-controller                                         waiting for a volume to be created, either by external provisioner "csi.cio.storidge.com" or manually created by system administrator
  Normal   Provisioning           2m57s  csi.cio.storidge.com_kworker1_2097513b-de84-4edb-80f7-2c75277ecdf3  External provisioner is provisioning volume for claim "default/nginx-pvc"
  Normal   ProvisioningSucceeded  2m55s  csi.cio.storidge.com_kworker1_2097513b-de84-4edb-80f7-2c75277ecdf3  Successfully provisioned volume pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb
```

Resize pvc
```
# nano nginx-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  accessModes:
  - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: cio-nginx

# kubectl apply -f nginx-pvc.yaml
persistentvolumeclaim/nginx-pvc configured
```

Confirm pvc resize
```
# kubectl get pvc
NAME        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
nginx-pvc   Bound    pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb   5Gi        RWO            cio-nginx      2m02s
```

Show pv details
```
# kubectl describe pv
Name:            pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb
Labels:          <none>
Annotations:     pv.kubernetes.io/provisioned-by: csi.cio.storidge.com
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    cio-nginx
Status:          Bound
Claim:           default/nginx-pvc
Reclaim Policy:  Delete
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        1Gi
Node Affinity:   <none>
Message:
Source:
    Type:              CSI (a Container Storage Interface (CSI) volume source)
    Driver:            csi.cio.storidge.com
    VolumeHandle:      pvc-bf1a3b06-30b2-4f6d-b0d6-45f3e11d9aeb
    ReadOnly:          false
    VolumeAttributes:      storage.kubernetes.io/csiProvisionerIdentity=1572577954725-8081-csi.cio.storidge.com
Events:                <none>
```

### Create pod to consume pvc

```
# cat nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: frontend
spec:
  containers:
   - name: nginx-server
     image: nginx
     ports:
       - containerPort: 80
         name: "http-server"
     volumeMounts:
       - name: nginx-vol
         mountPath: /usr/share/nginx/html
  volumes:
   - name: nginx-vol
     persistentVolumeClaim:
       claimName: nginx-pvc
       readOnly: false
```

```
# kubectl create -f nginx-pod.yaml
pod/nginx created
```

Confirm pod creation
```
# kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          38s
```

Login and validate mount point
```
# kubectl exec -it nginx /bin/bash

root@nginx:/# mount | grep nginx
/dev/vdisk/vd1 on /usr/share/nginx/html type xfs (rw,relatime,attr2,inode64,noquota)

root@nginx:/# exit
exit
```

Delete pod and pvc
```
# kubectl delete pod nginx
pod "nginx" deleted

# kubectl delete pvc nginx-pvc
persistentvolumeclaim "nginx-pvc" deleted
```

## Examples - Snapshots

## Examples - Volume expansion

### PVC [YAML File](https://docs.storidge.com/kubernetes_storage/volume_capacity_expansion.html#expand-pvc-example) Example

To resize a Storidge PVC, simply edit the PVC spec and change the size, following the steps below. We'll use the example PVC spec (`pvc-test.yaml`) below to create a volume and then expand it. The StorageClass must have `allowVolumeExpansion: true`.

```

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-test
spec:
  accessModes:
  - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 11Gi
  storageClassName: cio-default

```

### Steps to Expand PVC

1. Run `kubectl apply -f pvc-test.yaml` to apply the PVC spec above and create a Storidge volume of size 11GiB.
2. To resize the volume, run `kubectl edit pvc pvc-test` and change the size in the PVC spec under resources->requests->storage to 22Gi.
3. Save the spec and confirm the resize operation by running `kubectl describe pvc pvc-test`. You should see an event like the entry below.

```

volume pvc-443b9c5a-d92f-4171-a937-7362470c0f68
  Normal   VolumeResizeSuccessful  20s (x2 over 50s)  external-resizer csi.cio.storidge.com                              Resize volume succeeded

```

### Automated Expansion

Storidge volumes support automatic capacity expansion to minimize operator effort, and the possibility of application disruptions. This eliminates the need to edit the PVC spec to change the size of a volume.

See the [auto expansion example](https://guide.storidge.com/getting_started/autoexpand.html) to enable this service.

## Examples - NFS/shared volumes

## Resources

- [Documentation](https://docs.storidge.com)
- [Get Started](https://guide.storidge.com)
- [FAQ](https://faq.storidge.com)

## Support

Please join our [community Slack channel](https://storidge.com/join-cio-slack) to post questions and get support.
