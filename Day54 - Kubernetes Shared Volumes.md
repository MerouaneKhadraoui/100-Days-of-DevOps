# Day 54: Kubernetes Shared Volumes

## Question

We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.

**Task:**

1. Create a pod named `volume-share-devops`.

2. For the first container, use image `ubuntu` with `latest` tag only and remember to mention the tag i.e `ubuntu:latest`, container should be named as `volume-container-devops-1`, and run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/beta`.

3. For the second container, use image `ubuntu` with the `latest` tag only and remember to mention the tag i.e `ubuntu:latest`, container should be named as `volume-container-devops-2`, and again run a `sleep` command for it so that it remains in running state. Volume `volume-share` should be mounted at path `/tmp/games`.

4. Volume name should be `volume-share` of type `emptyDir`.

5. After creating the pod, exec into the first container i.e `volume-container-devops-1`, and just for testing create a file `beta.txt` with any content under the mounted path of first container i.e `/tmp/beta`.

6. The file `beta.txt` should be present under the mounted path `/tmp/apps` on the second container `volume-container-devops-2` as well, since they are using a shared volume.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## ✅ YAML Solution

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
    - name: volume-container-devops-1
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/beta

    - name: volume-container-devops-2
      image: ubuntu:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps
  volumes:
    - name: volume-share
      emptyDir: {}
```

## ⚡ Steps to Test

1. **Apply the pod manifest**

```bash
kubectl apply -f volume-share-devops.yaml
```

---

2. **Check if the pod is running**

```bash
kubectl get pods
```

---

3. **Exec into the first container and create the file**

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- bash
echo "Hello from container1" > /tmp/beta/beta.txt
exit
```

---

4. **Exec into the second container and check the shared file**

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- bash
cat /tmp/apps/beta.txt
exit
```

👉 You should see the content `"Hello from container1"`.

---

## 🔑 Key Points

- Both containers mount the same `emptyDir` volume but at **different mount paths** (`/tmp/beta` and `/tmp/apps`).
- `emptyDir` volumes are **ephemeral** → they exist as long as the Pod is running.
- Any file created in one container inside the volume will immediately be visible to the other.
