# Day 60: Persistent Volumes in Kubernetes

## Question

The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

**Task:**

1. Create a `PersistentVolume` named as `pv-devops`. Configure the `spec` as storage class should be `manual`, set capacity to `4Gi`, set access mode to `ReadWriteOnce`, volume type should be `hostPath` and set path to `/mnt/data` (this directory is already created, you might not be able to access it directly, so you need not to worry about it).

2. Create a `PersistentVolumeClaim` named as `pvc-devops`. Configure the `spec` as storage class should be `manual`, request `1Gi` of the storage, set access mode to `ReadWriteOnce`.

3. Create a `pod` named as `pod-devops`, mount the persistent volume you created with claim name `pvc-devops` at document root of the web server, the container within the pod should be named as `container-devops` using image `nginx` with `latest` tag only (remember to mention the tag i.e `nginx:latest`).

4. Create a node port type service named `web-devops` using node port `30008` to expose the web server running within the pod.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Create the YAML file**

Run this on the `jump_host`:

```bash
vi devops.yaml
```
Paste the following content:

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops
spec:
  storageClassName: manual
  capacity:
    storage: 4Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
  labels:
    app: devops
spec:
  containers:
    - name: container-devops
      image: nginx:latest
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: devops-storage
  volumes:
    - name: devops-storage
      persistentVolumeClaim:
        claimName: pvc-devops
---
apiVersion: v1
kind: Service
metadata:
  name: web-devops
spec:
  type: NodePort
  selector:
    app: devops
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
```

---

2. **Apply**

```bash
kubectl apply -f devops.yaml
```

---

3. **Verify**

```bash
kubectl get pv,pvc,pod,svc
```

---

4. **Test access in browser or curl**

```bash
curl http://<NodeIP>:30008
```