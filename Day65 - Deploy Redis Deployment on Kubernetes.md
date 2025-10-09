# Day 65: Deploy Redis Deployment on Kubernetes

## Question

The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

**Task:**

Create a redis deployment with following parameters:

1. Create a `config map` called `my-redis-config` having `maxmemory 2mb` in `redis-config`.

2. Name of the `deployment` should be `redis-deployment`, it should use
`redis:alpine` image and container name should be `redis-container`. Also make sure it has only `1` replica.

3. The container should request for `1` CPU.

4. Mount `2` volumes:

  a. An Empty directory volume called `data` at path `/redis-master-data`.

  b. A configmap volume called `redis-config` at path `/redis-master`.

  c. The container should expose the port `6379`.

5. Finally, `redis-deployment` should be in an up and running state.

`Note:` 

The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

---

## 🧩 Step 1: Create the ConfigMap

Create a ConfigMap named `my-redis-config` containing the Redis configuration.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-redis-config
data:
  redis-config: |
    maxmemory 2mb
```
✅ This creates a ConfigMap with a single key (`redis-config`) and value (`maxmemory 2mb`).

## ⚙️ Step 2: Create the Redis Deployment

Now create the deployment manifest file `redis-deployment.yaml`:

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
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "1"
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config
          items:
          - key: redis-config
            path: redis.conf
```
✅ This defines:

- Deployment name: `redis-deployment`
- Image: `redis:alpine`
- Container name: `redis-container`
- CPU request: `1`
- Port exposed: `6379`
- Volumes:
  - EmptyDir → `/redis-master-data`
  - ConfigMap → `/redis-master/redis.conf`

---

## 🚀 Step 3: Verify the Deployment

Run the following commands to confirm everything is working:

```bash
kubectl get deployments
kubectl get pods
kubectl describe pod -l app=redis
```
You should see 1 running replica.

---

## 🧠 (Optional) Step 4: Verify Redis Config Mounted Correctly

```bash
kubectl exec -it $(kubectl get pod -l app=redis -o jsonpath='{.items[0].metadata.name}') -- cat /redis-master/redis.conf
```
You should see:

```bash
maxmemory 2mb
```
---

## ✅ Final Result:

A fully functional Redis Deployment named `redis-deployment` using the `redis:alpine` image, with configuration loaded from `my-redis-config`, and persistent data stored in an emptyDir volume.