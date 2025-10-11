# Day 67: Deploy Guest Book App on Kubernetes

## Question

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

**Task:**

`BACK-END TIER`

1. Create a deployment named `redis-master` for Redis master.

  a. Replicas count should be `1`.

  b. Container name should be `master-redis-xfusion` and it should use image `redis`.

  c. Request resources as `CPU` should be `100m` and `Memory` should be `100Mi`.

  d. Container port should be redis default port i.e `6379`.

2. Create a service named `redis-master` for Redis master. Port and targetPort should be Redis default port i.e `6379`.

3. Create another deployment named `redis-slave` for Redis slave.

  a. Replicas count should be `2`.

  b. Container name should be `slave-redis-xfusion` and it should use `gcr.io/google_samples/gb-redisslave:v3` image.

  c. Requests resources as `CPU` should be `100m` and `Memory` should be `100Mi`.

  d. Define an environment variable named `GET_HOSTS_FROM` and its value should be `dns`.

  e. Container port should be Redis default port i.e `6379`.

4. Create another service named `redis-slave`. It should use Redis default port i.e `6379`.

`FRONT END TIER`

1. Create a deployment named `frontend`.

  a. Replicas count should be `3`.

  b. Container name should be `php-redis-xfusion` and it should use `gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff` image.

  c. Request resources as `CPU` should be `100m` and `Memory` should be `100Mi`.

  d. Define an environment variable named as `GET_HOSTS_FROM` and its value should be `dns`.

  e. Container port should be `80`.

2. Create a service named `frontend`. Its type should be `NodePort`, port should be `80` and its `nodePort` should be `30009`.

Finally, you can check the `guestbook app` by clicking on `App` button.


`You can use any labels as per your choice.`

`Note:` 

The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.

---

## 🧱 Step 1 – PersistentVolume (`mysql-pv.yaml`)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /var/lib/mysql
```

## 🧱 Step 2 – PersistentVolumeClaim (`mysql-pv-claim.yaml`)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```

---

## 🔐 Step 3 – Secrets (`mysql-secrets.yaml`)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: Opaque
data:
  password: WVVJaWRoYjY2Nw==   # base64 for YUIidhb667

---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: Opaque
data:
  username: a29kZWtsb3VkX3JveQ==   # base64 for kodekloud_roy
  password: R3lRa0ZSVk5yMw==       # base64 for GyQkFRVNr3

---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
data:
  database: a29kZWtsb3VkX2RiNw==   # base64 for kodekloud_db7
```

✅ Note: You can verify these encodings using
`echo -n 'YUIidhb667' | base64` etc.

---

## 🚀 Step 4 – Deployment (`mysql-deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
```

---

## 🌐 Step 5 – Service (`mysql-service.yaml`)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007
```
---

## ✅ Step 6 – Apply All

Run these commands in order:

```bash
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-pv-claim.yaml
kubectl apply -f mysql-secrets.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
```
---

## 🔍 Verify Deployment

```bash
kubectl get pv
kubectl get pvc
kubectl get secrets
kubectl get pods
kubectl get svc
```
Once the MySQL pod is running, you can connect using:

```bash
kubectl exec -it <mysql-pod-name> -- mysql -u kodekloud_roy -p
```
(Password: `GyQkFRVNr3`)