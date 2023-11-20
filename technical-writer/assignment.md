# Monitor and Debug in Kubernetes

Even the most experienced Spectro Cloud practitioners face issues with Kubernetes. This page references common `kubectl` commands to help you monitor and debug Kubernetes resources.

For a more in-depth overview, read the official Kubernetes guide on [Monitoring, Logging, and Debugging](https://kubernetes.io/docs/tasks/debug/).

> **Tip:**
> If you're relatively new to Kubernetes and want to practice monitoring and debugging in a safe, isolated environment, follow the [Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/) tutorial first to deploy and run a sample app.

> **Reminder:**
> You can append `-h` to each command to display more options and examples in your terminal.
>
> ```shell
> kubectl get -h
> ```
>
> You can also review the [official documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-) on `kubectl` to get more details about the commands we recommend and learn about other `kubectl` capabilities.

## Commands for Monitoring

### `kubectl get`

Get a quick snapshot of the basic resource information within the current or specific [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

```shell
kubectl get [(-o|--output=)json|yaml|wide|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...] (TYPE [NAME | -l label] | TYPE/NAME ...) [flags]
```

Review the following snippet for common `kubectl get` examples.

```shell
# Get all pods in the current namespace
kubectl get pods
# Get a specific pod in the current namespace
kubectl get pod <pod-name>
# Get all pods in a specific namespace
kubectl get pods --namespace <namespace-name>
# Get all pods from all namespaces
kubectl get pods --all-namespaces
# Get several resource groups
kubectl get pods,services
# Get resource info in a specific output format
kubectl get pod <pod-name> -o yaml
```

---

### `kubectl describe`

Output a detailed overview of specific resources, including their name, configuration, containers, events, etc.

```shell
kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [options]
```

Review the following snippet for common `kubectl describe` examples.

```shell
# Describe all pods in the current namespace
kubectl describe pods
# Describe a specific pod in the current namespace
kubectl describe pod <pod-name>
# Describe all resources of a specific type within a specific namespace
kubectl describe pods --namespace <namespace-name>
# Describe resources of a specific type that match a label selector
kubectl describe pods -l <label-selector>
# Describe a specific service
kubectl describe service <service-name>
```

---

### `kubectl top`

Monitor the CPU and memory metrics for nodes and pods within a cluster.

```shell
kubectl top [flags] [options]
```

Review the following snippet for common `kubectl top` examples.

```shell
# Display metrics for all nodes
kubectl top node
# Display metrics for a specific node
kubectl top node <node-name>
# Display metrics for all pods in the current namespace
kubectl top pod
# Display metrics for a specific pod in the current namespace
kubectl top pod <pod-name>
# Display metrics for all pods from all namespaces
kubectl top pod --all-namespaces
# Display metrics for resources of a specific type that match a label selector
kubectl top pod -l <label-selector>
```

---

### `kubectl cluster-info`

Display addresses of your cluster's control pane and services to analyze cluster health and control pane components.

```shell
kubectl cluster-info [flags] [options]
```

Review the following snippet for common `kubectl cluster-info` examples.

```shell
# Get basic cluster diagnostics
kubectl cluster-info
# Dump all cluster diagnostics
kubectl cluster-info dump
# Get diagnostics for a specific namespace
kubectl cluster-info dump --namespace <namespace-name>
```

## Commands for Debugging

### `kubectl logs`

Output logs from different resources to understand their runtime behavior and potential errors.

```shell
kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER] [options]
```

Review the following snippet for common `kubectl logs` examples.

```shell
# Get logs from a specific pod
kubectl logs <pod-name>
# Get logs from a specific pod in a specific namespace
kubectl logs <pod-name> --namespace <namespace-name>
# Stream logs from a specific pod
kubectl logs <pod-name> -f
# Get logs from a specific container within a pod
kubectl logs <pod-name> -c <container-name>
# Get the last 100 lines of logs from a specific pod
kubectl logs <pod-name> --tail=100
# Get logs from a specific pod for the last hour
kubectl logs <pod-name> --since=1h
```

---

### `kubectl debug`

Create a temporary container to debug resources without affecting the original environment.

```shell
kubectl debug (POD | TYPE[[.VERSION].GROUP]/NAME) [ -- COMMAND [args...] ] [options]
```

Review the following snippet for common `kubectl debug` examples.

```shell
# Create a debug container in a running pod
kubectl debug <pod-name> -it --image=busybox --container=debug-container
# Create a debug container with specific environment variables
kubectl debug <pod-name> --image=busybox --env="KEY=value"
# Copy a specific pod for debugging
kubectl debug <pod-name> --copy-to=<debug-pod-name> --image=busybox
# Start an interactive shell session to issue commands in the new debug container
kubectl debug <pod-name> -it --image=busybox -- sh
```

---

### `kubectl exec`

Gain direct access to a specific resource and issue commands to diagnose and troubleshoot its behavior.

```shell
kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...] [options]
```

Review the following snippet for common `kubectl exec` examples.

```shell
# List directory contents in a specific pod
kubectl exec <pod-name> -- ls /<dir-name>
# List directory contents in a specific container within a pod
kubectl exec <pod-name> -c <container-name> -- ls /<dir-name>
# List directory contents in a pod within a specific namespace
kubectl exec <pod-name> --namespace <namespace-name> -- ls /<dir-name>
# Start an interactive shell session within a pod
kubectl exec -it <pod-name> -- /bin/bash
```

---

### `kubectl attach`

Interact with active processes inside a container.

```shell
kubectl attach (POD | TYPE/NAME) -c CONTAINER [options]
```

Review the following snippet for common `kubectl attach` examples.

```shell
# Attach to a container within a pod
kubectl attach <pod-name> -c <container-name>
# Start an interactive shell session within the first pod container
kubectl attach <pod-name> -it
# Start an interactive shell session within a specific container
kubectl attach <pod-name> -c <container-name> -it
```

---

### `kubectl port-forward`

Forward one or more local ports to a pod to expose it locally for debugging.

```shell
kubectl port-forward TYPE/NAME [options] [LOCAL_PORT:]REMOTE_PORT [...[LOCAL_PORT_N:]REMOTE_PORT_N]
```

Review the following snippet for common `kubectl port-forward` examples.

```shell
# Forward a single port
kubectl port-forward <pod-name> 8080:80
# Forward a port to a specific local address
kubectl port-forward --address 0.0.0.0 <pod-name> 8080:80
# Forward a port to a pod within a specific namespace
kubectl port-forward --namespace <namespace-name> <pod-name> 8080:80
```
