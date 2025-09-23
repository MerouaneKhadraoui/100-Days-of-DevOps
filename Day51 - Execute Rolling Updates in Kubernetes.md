# Day 51: Execute Rolling Updates in Kubernetes

## Question

An application currently running on the Kubernetes cluster employs the nginx web server. The Nautilus application development team has introduced some recent changes that need deployment. They've crafted an image `nginx:1.18` with the latest updates.

**Task:**

Execute a rolling update for this application, integrating the `nginx:1.18` image. The deployment is named `nginx-deployment`.

Ensure all pods are operational post-update.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Check the deployment first**

```bash
kubectl get deployments
```
Ensure `nginx-deployment` exists.

---

2. **Check pods running from the deployment**

```bash
kubectl get pods -l app=nginx
```

---

3. **Perform the rolling update by setting a new image**

```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.18
```
---

- Here `nginx-container` is the container name inside the deployment (same as the image in most KodeKloud labs).

4. **Verify rollout status**

```bash
kubectl rollout status deployment/nginx-deployment
```
---

5. **Check pods again after update**

```bash
kubectl get pods
```
---

6. **Optional – describe to confirm image**

```bash
kubectl describe deployment nginx-deployment | grep Image
```
---

## 🔎 Final check

- All pods should be running with image nginx:1.18.
- Rollout should show successfully rolled out.

---

👉 In one compact command sequence for KodeKloud labs:

```bash
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.18
kubectl rollout status deployment/nginx-deployment
kubectl get pods
```