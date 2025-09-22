# Day 50: Set Resource Limits in Kubernetes Pods

## Question

The Nautilus DevOps team has noticed performance issues in some Kubernetes-hosted applications due to resource constraints. To address this, they plan to set limits on resource utilization. Here are the details:

**Task:**

Create a pod named `httpd-pod` with a container named `httpd-container`. Use the `httpd` image with the `latest` tag (specify as `httpd:latest`). Set the following resource limits:

Requests: Memory: `15Mi`, CPU: `100m`

Limits: Memory: `20Mi`, CPU: `100m`

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Create the YAML file**

```bash
vi httpd-pod.yaml
```
Paste the above content and save.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
```
---

2. **Apply the configuration**

```bash
kubectl apply -f httpd-pod.yaml
```

---

3. **Verify the pod**

```bash
kubectl get pods
kubectl describe pod httpd-pod
```
---

This will ensure the container has:

- Requests: minimum guaranteed resources (`15Mi` memory, `100m` CPU)
- Limits: maximum allowed usage (`20Mi` memory, `100m` CPU)