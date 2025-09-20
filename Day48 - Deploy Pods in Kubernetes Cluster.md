# Day 48: Deploy Pods in Kubernetes Cluster

## Question

The Nautilus DevOps team is diving into Kubernetes for application management. One team member has a task to create a pod according to the details below:

**Task:**

1. Create a pod named `pod-nginx` using the `nginx` image with the `latest` tag. Ensure to specify the tag as `nginx:latest`.

2. Set the `app` label to `nginx_app`, and name the container as `nginx-container`.

`Note:` The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

## Solution

1. **Create a Pod manifest file**

You can create a YAML file (recommended way):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
    app: nginx_app
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
```
Save it as pod-nginx.yaml.

---

2. **Apply the manifest**

```bash
kubectl apply -f pod-nginx.yaml
```

---

3. **Verify pod creation**

```bash
kubectl get pods -o wide
kubectl describe pod pod-nginx
```

---

👉 That’s it! Your pod `pod-nginx` with container `nginx-container` and label `app=nginx_app` will be running in the cluster.