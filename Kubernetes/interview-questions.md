# Kubernetes Interview Questions

**1. Explain Kubernetes/K8s architecture.**

- **Control Plane**

  - **kube-apiserver:** The primary interface for managing the Kubernetes
    cluster. Handles API requests, manages state, and
    authentication/authorization.
  - **kube-scheduler:** Selects nodes to run pods based on resource availability
    and policies.
  - **kube-controller-manager:** Manages Kubernetes controllers responsible for
    maintaining the cluster's desired state (e.g., Node Controller, Replication
    Controller).
  - **etcd:** Distributed key-value store holding the cluster's configuration
    and state.
  - **cloud-controller-manager:** Manages interactions with the cloud provider's
    API.

- **Node**
  - **kubelet:** Agent running on each node, responsible for managing containers
    according to Pod specifications.
  - **kube-proxy:** Handles network connectivity and load balancing within the
    cluster.
  - **Container Runtime:** Executes containers (e.g., Docker, containerd).

**2. What is Kubernetes Kops?**

- **kops** is a command-line tool for creating, managing, and deleting
  production-ready Kubernetes clusters, primarily on AWS (other cloud providers
  partially supported). It automates cluster provisioning and simplifies
  management.

**3. Explain the Replication Controller in Kubernetes.**

- **Replication Controllers** manage the replication of pods, ensuring a
  specified number of pod replicas run at all times. If a pod fails, the
  Replication Controller automatically launches new pods to maintain the desired
  replica count.

**4. What are PV and PVC in Kubernetes? What role do they play?**

- **PV (Persistent Volume):** A storage unit provisioned by an administrator,
  representing a portion of available storage in the cluster, independent of pod
  lifecycles.
- **PVC (Persistent Volume Claim):** A request by a pod to use a specific amount
  of storage. Kubernetes dynamically matches PVCs with PVs based on size and
  access modes.

**5. A pod is trying to access a volume, but it gives an access error. We would
like this pod to have access to this volume. What can we do to achieve the
same?**

- **Diagnose:**
  - Verify the PVC and PV are bound.
  - Check access modes and permissions.
  - Inspect pod and volume mount configurations.
- **Resolution:**
  - If unbound, create/modify a PVC to match the PV.
  - Adjust access modes if mismatch exists.
  - Ensure the pod's PodSpec correctly mounts the volume.

**6. What is a sidecar container?**

- A **sidecar container** is a supplementary container packaged alongside the
  main application container within a pod. Sidecars extend functionality without
  modifying the primary application, handling tasks like logging, configuration
  syncing, or data proxying.

**7. We want to launch certain pods only on specific nodes. These nodes should
be explicitly used only for these pods. Is it possible to control the deployment
of pods in such a scenario?**

- Yes, this uses:
  - **Node Labels:** Assign identifying key-value pairs to nodes.
  - **Node Affinity:** Include in PodSpecs to specify preferences or hard
    requirements for scheduling on nodes with particular labels.

**8. How does the K8s scheduler quickly assign worker nodes for the pods?
Explain the internal working of the K8s scheduler.**

- **Scheduling Process:**
  - **Filtering:** Filters out nodes that don't meet the pod's basic
    requirements (resources, nodeSelectors).
  - **Scoring:** Ranks remaining nodes based on factors like resource
    utilization and pod priorities.
  - **Binding:** Assigns the pod to the highest-scoring node.

**9. You have 10 nodes with 500GB of disk attached to each node. Let's say the
node space reaches 85% usage; then the pod we have deployed should get evicted
and re-deployed on a node with better disk health. Explain if this can even be
done?**

- Yes, this is achievable using:
  - **Resource Requests/Limits:** Set appropriate resource limits on pods to
    prevent overconsumption.
  - **Eviction Policies:** Configure eviction thresholds based on disk usage.
    Kubernetes will evict pods when thresholds are breached.
  - **Pod Disruption Budgets (PDBs):** Define how many pods of an application
    can be unavailable during disruptions like node eviction.
