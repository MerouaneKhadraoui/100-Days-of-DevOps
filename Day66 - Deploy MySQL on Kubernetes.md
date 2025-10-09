# Day 66: Deploy MySQL on Kubernetes

## Question

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:

**Task:**

1. Create a PersistentVolume `mysql-pv`, its capacity should be `250Mi`, set other parameters as per your preference.

2. Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as `mysql-pv-claim` and request a `250Mi` of storage. Set other parameters as per your preference.

3. Create a deployment named `mysql-deployment`, use any mysql image as per your preference. Mount the PersistentVolume at mount path `/var/lib/mysql`.

4. Create a `NodePort` type service named `mysql` and set nodePort to `30007`.

5. Create a secret named `mysql-root-pass` having a key pair value, where key is `password` and its value is `YUIidhb667`, create another secret named `mysql-user-pass` having some key pair values, where frist key is `username` and its value is `kodekloud_roy`, second key is password and value is `GyQkFRVNr3`, create one more secret named `mysql-db-url`, key name is `database` and value is `kodekloud_db7`

6. Define some Environment variables within the container:

  a. `name: MYSQL_ROOT_PASSWORD`, should pick value from secretKeyRef `name: mysql-root-pass` and `key: password`

  b. `name: MYSQL_DATABASE`, should pick value from secretKeyRef name: `mysql-db-url` and `key: database`

  c. `name: MYSQL_USER`, should pick value from secretKeyRef `name: mysql-user-pass` key `key: username`

  d. `name: MYSQL_PASSWORD`, should pick value from secretKeyRef `name: mysql-user-pass` and `key: password`

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