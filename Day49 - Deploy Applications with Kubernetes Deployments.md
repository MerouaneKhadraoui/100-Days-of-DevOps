# Day 49: Deploy Applications with Kubernetes Deployments

## Question

The Nautilus DevOps team is delving into Kubernetes for app management. One team member needs to create a deployment following these details:

**Task:**

Create a deployment named `httpd` to deploy the application `httpd` using the image `httpd:latest` (ensure to specify the tag)

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## ✅ Option 1: Create deployment directly with `kubectl`

```bash
kubectl create deployment httpd --image=httpd:latest
```

---

## ✅ Option 2: Using a YAML manifest (preferred for exams/interviews)

Create a file named httpd-deployment.yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd:latest
```
Then apply it:

```bash
kubectl apply -f httpd-deployment.yaml
```

---

## ✅ Verify deployment

```bash
kubectl get deployments
kubectl get pods
```
You should see a deployment called httpd and at least one pod running with the `httpd:latest` image.