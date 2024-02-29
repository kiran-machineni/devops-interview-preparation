## 1. Explain kubernetes/k8s architecture?

> Kubernetes Architecture:

### Control plane

- Controller manager
- API server
- Scheduler
- etcd
- Cloud control manager

### Node

- kubelet
- kube-procy
- container runtime

#### kube-controller manager

> the Controller manager is a daemon in control plane communicates with API server to determine the node, object's state.
if the current state is different from desired state, the controller manager makes changes attempting to move the current state towards the desired state.

> It maintains a set of non-terminating  control loops, each associated with a specific type of resource (e.g., Pods, Deployments, Services).

##### different controllers in kube-controller manager

- <b>Node controller</b>
- Namespace controller
- Service controller
- Service account and Tocken controller
- Endpoint controller
- ReplicaSet controller
- <b>DeploymentSet</b> controller
- StatefulSet controller
- DeamonSet controller
- Persistent volume controller and persistent volume bind
- Secret and configMap Controller
- Job controller
- Cron job controller
