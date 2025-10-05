# Day 62: Manage Secrets in Kubernetes

## Question

The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

**Task:**

1. We already have a secret key file `ecommerce.txt` under `/opt` location on `jump host`. Create a `generic secret` named `ecommerce`, it should contain the password/license-number present in `ecommerce.txt` file.

2. Also create a `pod` named `secret-nautilus`.

3. Configure pod's `spec` as container name should be `secret-container-nautilus`, image should be `ubuntu` with `latest` tag (remember to mention the tag with image). Use `sleep` command for container so that it remains in running state. Consume the created secret and mount it under `/opt/apps` within the container.

4. To verify you can exec into the container `secret-container-nautilus`, to check the secret key under the mounted path `/opt/apps`. Before hitting the `Check` button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## 🧩 Step 1: Create the secret from the file

We’re told the file `/opt/ecommerce.txt` exists on the **jump host**, and we must create a **generic secret** named `ecommerce` from it.

```bash
kubectl create secret generic ecommerce --from-file=/opt/ecommerce.txt
```
✅ This creates a secret named `ecommerce` whose key will automatically be `ecommerce.txt` (unless you rename it).

You can verify:

```bash
kubectl get secret ecommerce
kubectl describe secret ecommerce
```

## 🧱 Step 2: Create the Pod manifest

Now we’ll create a Pod named `secret-nautilus` with:

- container name: `secret-container-nautilus`
- image: `ubuntu:latest`
- command: `sleep infinity` (to keep it running)
- secret mounted at `/opt/apps`

Let’s create a YAML file:

```bash
vi secret-pod.yaml
```
Paste this content:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: secret-nautilus
spec:
  containers:
    - name: secret-container-nautilus
      image: ubuntu:latest
      command: ["sleep", "infinity"]
      volumeMounts:
        - name: secret-volume
          mountPath: /opt/apps
  volumes:
    - name: secret-volume
      secret:
        secretName: ecommerce
```
Then apply it:

```bash
kubectl apply -f secret-pod.yaml
```

---

## ⚙️ Step 3: Verify the Pod is running

Check the pod status:

```bash
kubectl get pods
```
Wait until it’s in the **Running** state.

---

## 🔍 Step 4: Verify the secret is mounted

Exec into the container:

```bash
kubectl exec -it secret-nautilus -- bash
```
Inside the container, list the files:

```bash
ls /opt/apps
```

You should see:

```css
ecommerce.txt
```
View the secret content (it’ll be base64-decoded automatically by Kubernetes):

```bash
cat /opt/apps/ecommerce.txt
```
That should display the password/license number from your `/opt/ecommerce.txt` file on the jump host.