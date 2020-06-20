#### Understand Kubernetes Cluster upgrade process
```bash
Allowed combination of kubernetes binary versions:

kube-apiserver - version X
controller-manager - version X-1
kube-scheduler - version X-1
kubelet - version X-2
kube-proxy - version X-2
kubectl - version X+1 > X-1

Control plane upgrade
***************
apt update
apt-cache madison kubeadm
# find the latest 1.18 version in the list
# it should look like 1.18.x-00, where x is the latest patch
# replace x in 1.18.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.18.x-00 && \
apt-mark hold kubeadm
kubectl drain <node_name> --ignore-daemonsets
kubeadm upgrade plan
# for additional control plane upgrades, do `kubeadm upgrade node` instead of the below command
kubeadm upgrade apply v1.18.x
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.18.x-00 kubectl=1.18.x-00 && \
apt-mark hold kubelet kubectl
systemctl restart kubelet
kubectl uncordon <node_name>

Worker nodes upgrade
**************
# replace x in 1.18.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.18.x-00 && \
apt-mark hold kubeadm
kubectl drain <node_name> --ignore-daemonsets
kubeadm upgrade node
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.18.x-00 kubectl=1.18.x-00 && \
apt-mark hold kubelet kubectl
systemctl restart kubelet
kubectl uncordon <node_name>

# to get the status of the nodes,
kubectl get nodes
```
Recovering from failed state - https://v1-18.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#recovering-from-a-failure-state

#### Facilitate OS upgrades
```bash
# prepare for maintenance on a node
kubectl drain node1 --ignore-daemonsets
# Apply patches on node1, reboot etc and once everything is done, uncordon it
kubectl uncordon node1

# If you just want to make the node unschedulable but don't want to evict 
# the running pods then just cordon the node
kubectl cordon node1

# If you are running a pod on the node which is not part of a replicaset or controller then you need to force
# the eviction and that pod will be lost forever
kubectl drain node1 --ignore-daemonsets --force
```

ETCD upgrade - https://v1-18.docs.kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/

#### Backup

In a k8s cluster there are two main pieces of data that need to be backed up:
- Certificate Files
- Etcd database

```bash
# store all yamls
kubectl get all --all-namespaces -o yaml > all-services.yaml

# certificates
# back up the entire /etc/kubernetes/pki directory

# snapshot etcd
ETCDCTL_API=3 etcdctl snapshot save snapshot.db \
--cacert=/etc/etcd/ca.crt \
--cert=/etc/etcd/etcd-server.crt \
--key=/etc/etcd/etcd-server.key

# verify snapshot
ETCDCTL_API=3 etcdctl snapshot status snapshot.db
```
