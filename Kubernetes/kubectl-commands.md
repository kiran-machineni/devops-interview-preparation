# Kubernetes CLI Commands

## 1. Kubernetes Context

- **Display current context:**

  ```bash
  kubectl config current-context
  ```

- **Change context:**

  ```bash
  kubectl config use-context test
  ```

  - Switches the active Kubernetes context to "test".
  - A Kubernetes context combines cluster, user, and namespace details.

## 2. Kubernetes Manifests

### Understanding Manifests

Kubernetes manifests are YAML or JSON files defining the desired state of
objects within a Kubernetes cluster. Essential components of a manifest include:

- **Metadata:** Object name, labels, annotations for identification and
  organization.
- **Spec:** The desired state specification including container images,
  replicas, configurations.
- **Status:** (Read-only) Current state of the object, provided by Kubernetes.

### Key Manifest Actions

**2.1 Validation**

- **Syntax and Semantic Validation:**

  ```bash
  kubectl apply --validate=true -f deployment.yaml
  ```

- **Dry Run (Client-Side Simulation):**
  ```bash
  kubectl apply --dry-run=client -f deployment.yaml
  ```

**2.2 Applying Manifests**

- **Apply Specific Objects:**

  ```bash
  kubectl apply -f pod.yaml          # Pod
  kubectl apply -f deployment.yaml   # Deployment
  kubectl apply -f service.yaml      # Service
  # ...and so on for configmaps, secrets, volumes
  ```

- **Apply Manifests in a Folder:**
  ```bash
  kubectl apply -f /path/to/your/folder
  ```

**2.3 Inspecting Kubernetes Objects**

```bash
kubectl get pods
kubectl get deployments
kubectl get svc
kubectl get secrets
kubectl get pv

# output into a file
kubectl get deployment nginx-deployment -o yaml > nginx-deployment.yaml

kubectl get <resource_type> <resource_name> --namespace=<namespace> -o yaml > local-file.yaml
```

**2.4 Edit Kubernetes Objects**

```bash
kubectl edit deployment -n kube-system coredns
```

**2.5 Delete Kubernetes Objects**

```bash
# Delete deployments that match a specific label selector
kubectl delete deployments -l app=redis

# Delete deployment by name
kubectl delete deployment frontend

# Delete service by name
kubectl delete service nodejs-app-service
```

**2.6 Scale Deployments**

```bash
kubectl scale deployment your-deployment --replicas=3
```

**2.7 Roll back to a previous version**

Rolling back any changes introduced in the latest deployment.

```bash
kubectl rollout undo deployment/your-deployment
```

**2.8 Deploy Resources Without Creating YAML Configurations**

```bash
# Create deployment without a manifest
kubectl create deployment nginx --image=nginx

# Create pod without a manifest
kubectl run nginx --image=nginx --restart=never
```

## 3. Kubernetes Namespaces

- **Virtual Clusters:** Namespaces function like virtual clusters within a
  physical Kubernetes cluster, enabling the logical division of resources.
- **Isolation:** Namespaces keep resources separate, preventing naming conflicts
  and promoting better organization in multi-user or multi-project environments.
- **Resource Management:** Namespaces allow for setting resource quotas (CPU,
  memory), ensuring fair allocation of cluster resources between different teams
  or projects.
- **Default Namespace:** Kubernetes automatically assigns resources to the
  `default` namespace when a namespace is not explicitly specified during
  creation.

**Important Note:** Deleting a namespace **permanently removes all resources
associated with it**. Handle with care!

```bash
# Create a namespace called 'test'
kubectl create namespace test

# List all namespaces
kubectl get namespaces

# Delete the 'test' namespace
kubectl delete namespace test

# Perform a test without deleting (highly recommended before deletion)
kubectl delete namespace <namespace-name> --dry-run=client

# Retrieve deployments within a namespace
kubectl get deployments -n <namespace>
```

## 4. Kubectl Describe

The `kubectl describe` command provides in-depth information about Kubernetes
resources. It gathers data from multiple API calls to paint a comprehensive
picture of the resource's state, configuration, associated events, and more.

- **Debugging:** Get detailed insights to troubleshoot issues with pods,
  services, nodes, etc.
- **Understanding State:** See the current status of a resource, including its
  configuration, resource requests, and any related events.

```bash
# Describe a specific node named 'node-1'
kubectl describe node node-1

# Describe all pods in the current namespace
kubectl describe pods

# Describe a specific pod named 'nginx'
kubectl describe pods/nginx
```

## 5. Pod

```bash
# Copy files into container in pod
kubectl cp /home/thor/index.php <pod_name>:/destination/inside/container/ -c nginx-container

# Get containers from pod
kubectl get pod <pod_name> -o jsonpath='{.spec.containers[*].name}'

# Access pods container terminal
kubectl exec -it <pod_name> -c nginx-container -- /bin/sh

# Change image of container in a pod
kubectl set image pod/<pod_name> <container_name>=httpd:latest

# Change the image of the container in DeploymentSet
kubectl set image deployment/nginx-deployment nginx-container=nginx:latest

# Port forward in a pod
kubectl port-forward pod/<pod_name> -n <namespace> <hostport>:<service port no. in cluster>

kubectl port-forward pod/time-check -n datacenter 8080:80
```

## 6. Kubernetes Services

- **Abstraction for Pods:** Services provide a stable network endpoint for
  accessing pods, which are often ephemeral. They manage routing and load
  balancing traffic to a set of pods.
- **Service Discovery:** Services enable discovery within a cluster, so
  applications can communicate without needing to track individual pod IPs.

**Service Types**

- **NodePort:** Exposes the service on a static port on each node of the
  cluster. External traffic can access the service through any node's IP and the
  specified port.
- **LoadBalancer:** Provisions an external cloud load balancer (if supported by
  your Kubernetes provider). Distributes traffic across pods.
- **ClusterIP:** The default type. Exposes the service internally within the
  cluster, providing a stable IP.
- **ExternalName:** Maps a service to a DNS name outside the cluster.

**Example:**

```bash
# simple-service.yaml

# https://kubernetes.io/docs/concepts/services-networking/service/
apiVersion: v1
kind: Service
metadata:
  name: service-simple-service
spec:
  type: ClusterIP
  selector:
    app: service-simple-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
---
#multi-port-service.yaml

# https://kubernetes.io/docs/concepts/services-networking/service/#multi-port-services
apiVersion: v1
kind: Service
metadata:
  name: multi-port-service-service
spec:
  selector:
    app: MyApp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 8080
    - name: https
      protocol: TCP
      port: 443
      targetPort: 8443
```

```bash
# Change Nodeport port number from cli
kubectl patch service your-service -p '{"spec": {"ports": [{"nodePort": 32165}]}}'
```

## 7. Port Forward

Port forwarding in Kubernetes creates a tunnel between your local machine and a
service (or pod) running within the cluster. This is particularly useful for
debugging and accessing services not normally exposed outside the cluster.

```bash
kubectl port-forward service/node-app-service 8080:80
```

**8080 (local port):** The port to access on your local machine
(http://localhost:8080).

**80 (service port):** The port of the service within the Kubernetes cluster.
When you access http://localhost:8080, the traffic is forwarded to the service
inside the cluster, which then directs it to the pods associated with that
service.

**Example Service Configuration**

```bash
# Example service
apiVersion: v1
kind: Service
metadata:
  name: appname-service
spec:
  type: NodePort
  selector:
    app: appname
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30011
```

**Key Ports**

**port:** The port used to access the service from within the cluster.
**targetPort:** The port on the pod where the application is listening.
**nodePort:** The port exposed for accessing the service from outside the
cluster.

**Summary**

**Cluster Internal:** The service is accessible on localhost:80. **External:**
The service is accessible on NODE_IP:30011 (where NODE_IP is any node's IP).

## 8. Kustomize

Kustomize is a configuration management for k8s, without using templating.
Kustomize comes with kubectl.

```bash
# Apply kustomization.yaml configuration
kubectl apply -k myapp/

# Cleaning up
kubectl delete -k myapp/
```

## 9. Nodes

```bash
# Set a node to be unschedulable
kubectl cordon node01

# Set a node to be schedulable
kubectl uncordon node01

# Check node status
kubectl get nodes

```
