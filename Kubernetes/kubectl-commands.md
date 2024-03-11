# Kubernetes CLI Commands

#### 1. Display the current Kubernetes Context

Displays the name of the current Kubernetes context in your kubeconfig file.

```bash
kubectl config current-context
```

#### 2. Kubernetes Manifests

##### Validate the manifest file

> It checks the configuration for syntactic and semantic correctness but does not make any changes to the cluster.

```bash
kubectl apply --validate=true -f deployment.yaml
```

##### Dry run

> This command performs a 'dry run' of the apply operation, simulating the process of applying the configuration to the cluster without modifying the cluster state. The --dry-run=client flag specifies that the dry run is performed on the client side. During this process, the configuration is validated, and the modified configuration that would be applied is printed, allowing users to preview the changes that would occur.

```bash
kubectl apply --dry-run=client -f deployment.yaml
```

##### Apply manifest files

```bash

# Apply pod
kubectl apply -f pod.yaml

# Apply deployments
kubectl apply -f deployment.yaml

# Apply service
kubectl apply -f service.yaml

# Apply configmap
kubectl apply -f configmap.yaml

# Apply secrets
kubectl apply -f secrets.yaml

# Apply persistent volume
kubectl apply -f pv.yaml

# Apply persistent volume claim
kubectl apply -f pvc.yaml
```

##### Apply all manifests in a folder to the Kubernetes cluster

```bash
kubectl apply -f /path/to/your/folder
```

##### Get kubernetes objects

```bash
# Get pods
kubectl get pods

# Get deployments
kubectl get deployments

# Get services
kubectl get svc

# Get nodes in a cluster
kubectl get nodes

# Get the secrets
kubectl get secrets

# Get the persistent volumes
kubectl get pv
```

##### Delete kubernetes objects

```bash
# Delete a deployment that match a specific label selector
kubectl delete deployment -l app=redis

# Delete deployment with name
kubectl delete deployment frontend

# Delete service with name
kubectl delete service nodejs-app-service
```

##### Scale deployments

```bash
kubectl scale deployment your-deployment --replicas=3
```

##### Roll back to a previous version

> This command is used to undo a recent deployment rollout in Kubernetes. This command reverts the specified deployment to the previous revision, effectively rolling back any changes introduced in the latest deployment.

```bash
kubectl rollout undo deployment/your-deployment
```

##### Deploy resources without creating yaml configurations

```bash
# Create deployment without manifest
kubectl create deployment nginx --image=nginx

# Create pod without manifest
kubectl run nginx --image=nginx --restart=never
```

#### 3. Kubernetes Namespaces

> In Kubernetes, a namespace is a virtual cluster inside a physical cluster. It is a way to divide cluster resources between multiple users (or projects, teams, applications, etc.) on the same cluster. Namespaces provide a scope for names and are intended for use in environments with many users spread across multiple teams or projects.

> Namespaces provide isolation, resource quotas and logical partitioning.
When you create an object in Kubernetes without specifying a namespace, the resources are created in <b>default</b> namespace.

> deleting namespace will delete all the resources in that namespace.

```bash
# Create namespace
kubectl create namespace test

# Get namespaces
kubectl get namespaces

# Delete namespace
kubectl delete namespace test

# Dry run before deleting
kubectl delete namespace <namespace-name> --dry-run=client

# Get deployments with namespace
kubectl get deployments -n <namespace>
```

#### 4. kubectl describe

> Show details of a specific resource or group of resources.
This command joins many API calls together to form a detailed description of a given resource or group of resources.

```bash
# Describe node
kubectl describe node node-1

# Describe all pods
kubectl describe pods

# Describe pod
kubectl describe pods/nginx
```

---

#### 5. Pod

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

```bash
# Port forward in service
kubectl port-forward service/node-app-service 8080:80
```

##### Change Nodeport of service in Kubernetes service

```bash
kubectl patch service your-service -p '{"spec": {"ports": [{"nodePort": 32165}]}}'
```

##### Get manifest configuration of kubernetes object

> Retrieve the YAML representation of a Kubernetes deployment. The output is then redirected to a file named nginx-deployment.yaml. This YAML file will contain the configuration details of the specified deployment.

```bash
kubectl get deployment nginx-deployment -o yaml > nginx-deployment.yaml
```

```bash
kubectl get <resource_type> <resource_name> --namespace=<namespace> -o yaml > local-file.yaml
```

---

#### 6. port-forward

```bash
kubectl port-forward service/node-app-service 8080:80
```

> When you use <i>kubectl port-forward<i> , it establishes a local port forwarding from your machine to service in the cluster.

> 8080 on your local machine will be forwarded to the port 80 of the service within the cluster. This is the port where the service is exposed.
When you access <http://localhost:8080> on your machine, it's equivalent to accessing the service within the Kubernetes cluster on port 80, which, in turn, directs the traffic to the pods' targetPort (8080 in this case).

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

> ports:
      - protocol: TCP
          port: 80(service port within the cluster)
          targetPort: 8080(containerPort in deployment) (pod port)
          nodePort: 30011(port exposed to outside cluster traffic)
> Inside the cluster, the service is accessible at localhost:80.
The service forwards traffic to the Pods on port 8080.
Externally, the service is accessible at NODE_IP:30011, where NODE_IP is the IP address of any of the cluster nodes.

> When you use port-forward, it will directly send traffic to the pod port.
---

#### 7. Kubernetes service

- NodePort (expose service on the static port on every node in the cluster)
- LoadBalancer (Cloud Load Balancer)
- ClusterIP (expose service internally in a cluster)
- ExternalName (which allows you to map a service to a DNS name)

---

#### 8. [kustomize](https://kustomize.io/)

kustomization is a configuration management for k8s, without using templating.
kustomization comes with kubectl.

```bash
# Apply kustomization.yaml configuration
kubectl apply -k myapp/
```

```bash
# Cleaning up
kubectl delete -k myapp/
```
