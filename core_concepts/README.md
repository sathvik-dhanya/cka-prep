# Core Concepts (19%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

#### Understanding k8s API primitives

Documentation - https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md

#### Kubernetes cluster architecture
- Master Nodes
    - Etcd (unless external)
    - Kube API server
    - Kube Scheduler
    - Kube Controller Manager
- Worker Nodes
    - Some sort of CNI (Flannel, NSX-T, etc)
    - Kube Proxy
    - Kubelet
    - Container runtime (Docker, RKT, containerd, etc)

**ETCD**

A consistent and highly available key-value store leveraged by Kubernetes to store cluster related data. ETCD can be located on the master nodes, or can be a separate cluster all together.

**Kube API Server**

Everything we do in Kubernetes goes through the API server. Think of this as the front door to the Kubernetes cluster. This runs on all master nodes, to which a load balancer can be placed in front of.

**Kube Scheduler**

Authority for determining which pods run on which nodes. Kube Scheduler will also respect any constraints or requirements imposed by the pod specification, such as any nodeselector configuration.

**Kube Controller Manager**

Primarily responsible for checking, validating and rectifying current and intended state within the cluster. This includes:
- React to node failure.
- React to when the desired number of pods in a deployment (replication controller) is not satisfied.
- Populate endpoints for services based on defined inclusion criteria (i.e. labels).

**CNI**

Responsible for facilitating pod communication. A k8s cluster cannot function with a CNI.

**Kube Proxy**

Maintains network rules and provides a level of abstraction by forwarding connections as appropriate.

**Kubelet**

The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the API server) and ensures that the containers described in those PodSpecs are running and healthy.

**Container Runtime**

Usually Docker or Containerd, this provides the platform for containers to run on the host.

**Services types**

ClusterIP - Exposes the service on an IP address that is only accessible internally. Unless explicitly defined in the service yaml file, this is the default type.

NodePort - Exposes the service on each worker node’s IP address (non docker bridge) over a static port. This is accessible externally from the cluster.

Load Balancer - Exposes the service externally using a cloud provider’s load balancer. As implied, the cloud provider must support this functionality.
