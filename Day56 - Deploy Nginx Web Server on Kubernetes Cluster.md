# Day 56: Deploy Nginx Web Server on Kubernetes Cluster

## Question

Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:

**Task:**

1. Create a deployment using `nginx` image with `latest` tag only and remember to mention the tag i.e `nginx:latest`. Name it as `nginx-deployment`. The container should be named as `nginx-container`, also make sure replica counts are `3`.

2. Create a NodePort type service named `nginx-service`. The nodePort should be `30011`.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Create the Deployment manifest (`nginx-deployment.yaml`)**

```bash
vi nginx-deployment.yaml
```
Paste the above content and save.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
---

2. **Create the Deployment manifest (`nginx-service.yaml`)**

```bash
vi nginx-service.yaml
```
Paste the above content and save.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```
---

3. **Apply both manifests**

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

---

4. **Verify the resources**

- Check the deployment & pods:

```bash
kubectl get deployments
kubectl get pods -o wide
# or
k get all
```
- Check the service:

```bash
kubectl get svc
```
Expected output should show `nginx-service` with `NodePort` `30011`.

---

5. **Test Access**

If you’re on the **jump_host**, get the worker node’s IP:

```bash
kubectl get nodes -o wide
```
Then test in browser or curl:

```bash
curl http://<Node_IP>:30011
```

You should see the default **nginx welcome page** 🎉