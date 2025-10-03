# Day 61: Init Containers in Kubernetes

## Question

There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

**Task:**

1. Create a `Deployment` named as `ic-deploy-xfusion`.

2. Configure `spec` as replicas should be `1`, labels `app` should be `ic-xfusion`, template's metadata lables `app` should be the same `ic-xfusion`.

3. The `initContainers` should be named as `ic-msg-xfusion`, use image `fedora` with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'echo Init Done - Welcome to xFusionCorp Industries > /ic/official'`. The volume mount should be named as `ic-volume-xfusion` and mount path should be `/ic`.

4. Main container should be named as `ic-main-xfusion`, use image `fedora` with `latest` tag and use command `'/bin/bash'`, `'-c'` and `'while true; do cat /ic/official; sleep 5; done'`. The volume mount should be named as `ic-volume-xfusion` and mount path should be `/ic`.

5. Volume to be named as `ic-volume-xfusion` and it should be an emptyDir type.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Create the YAML file**

Run this on the `jump_host`:

```bash
vi ic-deploy-xfusion.yml
```
Paste the following content:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-xfusion
  labels:
    app: ic-xfusion
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-xfusion
  template:
    metadata:
      labels:
        app: ic-xfusion
    spec:
      initContainers:
      - name: ic-msg-xfusion
        image: fedora:latest
        command: ["/bin/bash", "-c" ,"echo Init Done - Welcome to xFusionCorp Industries > /ic/official"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic
      containers:
      - name: ic-main-xfusion
        image: fedora:latest
        command: ["/bin/bash", "-c" ,"while true; do cat /ic/official; sleep 5; done"]
        volumeMounts:
        - name: ic-volume-xfusion
          mountPath: /ic
      volumes:
      - name: ic-volume-xfusion
        emptyDir: {}
```

---

2. **Apply**

```bash
kubectl apply -f ic-deploy-xfusion.yaml
```

---

3. **Verify the pod is running**

```bash
kubectl get pods
```

---

4. **Check logs to confirm it prints the message**

```bash
kubectl logs -f <pod-name>
```

You should see:

```css
Init Done - Welcome to xFusionCorp Industries
```
repeated every 5 seconds. 🚀