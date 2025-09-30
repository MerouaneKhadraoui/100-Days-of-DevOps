# Day 58: Deploy Grafana on Kubernetes Cluster

## Question

The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.

**Task:**

1. Create a deployment named `grafana-deployment-datacenter` using any grafana image for Grafana app. Set other parameters as per your choice.

2. Create `NodePort` type service with nodePort `32000` to expose the app.

`You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.`

`Note:` The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

## Solution

1. **Create the YAML file**

Run this on the `jump_host`:

```bash
vi grafana.yaml
```

Paste the following content:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-datacenter
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
        - name: grafana
          image: grafana/grafana:latest   # You can pin a version if needed
          ports:
            - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
  labels:
    app: grafana
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - port: 3000        # Service port
      targetPort: 3000  # Container port
      nodePort: 32000   # Required NodePort
```

---

2. **Apply it**

```bash
kubectl apply -f grafana.yaml
```

---

3. **Verify**

```bash
kubectl get pods
kubectl get svc grafana-service
```
It should go into **Completed** state (since it just runs echo once).

---

4. **Access Grafana in your browser**

```css
http://<NodeIP>:32000
```

✅ Expected Output:

```css
Welcome to Stratos Industries
```

👉 Default Grafana login is usually `admin/admin` (but you don’t need to log in for this task, just reach the login page).