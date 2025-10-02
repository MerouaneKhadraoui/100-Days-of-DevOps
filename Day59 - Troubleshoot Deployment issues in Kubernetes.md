# Day 59: Troubleshoot Deployment issues in Kubernetes

## Question

Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

**Task:**

The deployment name is `redis-deployment`. The pods are not in running state right now, so please look into the issue and fix the same.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

```bash
kubectl edit deploy redis-deployment
```
The Result is :

```yaml
spec:
  containers:
  - image: redis:alpin
    imagePullPolicy: IfNotPresent
    name: redis-container
.
.
  - configMap:
      defaultMode: 420
      name: redis-conig
    name: config
```

---

## 🔎 Problems in the Deployment YAML

From your snippet:

```yaml
containers:
- image: redis:alpin
  imagePullPolicy: IfNotPresent
  name: redis-container
```
1. **Image typo** → `redis:alpin` is wrong.
✅ Should be `redis:alpine`.

---

```yaml
- configMap:
    defaultMode: 420
    name: redis-conig
  name: config
```
---

2. **ConfigMap typo** → `redis-conig` is wrong.
✅ Should be `redis-config` (most likely intended name).

---

## ✅ Correct Deployment Structure

A proper Deployment should look like this (fixing both issues):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        imagePullPolicy: IfNotPresent
        volumeMounts:
        - name: config
          mountPath: /redis-master
      volumes:
      - name: config
        configMap:
          name: redis-config
          defaultMode: 420
```
---

## 🛠️ How to Fix

Since the task says use `kubectl edit`, here’s what you should do:

1. Run:

```bash
kubectl edit deploy redis-deployment
```

2. Fix these:

- Change
`image: redis:alpin` → `image: redis:alpine`

- Change
`name: redis-conig` → `name: redis-config`

3. Save and exit.

---

## 🔍 Verify

After saving:

```bash
kubectl rollout status deployment redis-deployment
kubectl get pods -l app=redis
kubectl describe pod "redis-pod-name"
```
You should see the pod running fine.