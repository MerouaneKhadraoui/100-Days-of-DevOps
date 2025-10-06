# Day 64: Fix Python App Deployed on Kubernetes Cluster

## Question

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.

**Task:**

1. The deployment name is `python-deployment-nautilus`, its using `poroko/flask-demo-app` image. The deployment and service of this app is already deployed.

2. nodePort should be `32345` and targetPort should be python flask app's default port.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

When run this :

```bash
kubectl describe deploy python-deployment-nautilus
```
The result is :

```yaml
Name:                   python-deployment-nautilus
Namespace:              default
Selector:               app=python_app
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
Pod Template:
  Labels:  app=python_app
  Containers:
   python-container-nautilus:
    Image:         poroko/flask-app-demo
    Port:          5000/TCP
```
When run this :

```bash
kubectl get services
```
The result is :

```bash
NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes                ClusterIP   10.96.0.1       <none>        443/TCP          47m
python-service-nautilus   NodePort    10.96.203.251   <none>        8080:32345/TCP   10m
```

---

## 🧩 Problem Summary

You have:

- A deployment called `python-deployment-nautilus`
- A service called `python-service-nautilus`

But the **app is not running**, even though both exist.

From the description:

- The deployment uses image `poroko/flask-app-demo` (note typo — should be `poroko/flask-demo-app`)
- The Flask app’s default port is `5000`
- The service nodePort should be `32345`
- The service targetPort should match the Flask app’s container port (`5000`)

---

## 🚨 Root Cause

1. Wrong image name in the deployment:
- It uses `poroko/flask-app-demo`
- Should be `poroko/flask-demo-app`
  (The typo `app-demo` → should be `demo-app`)

2. Service misconfiguration:
- The service exposes `8080:32345/TCP`, but the Flask app listens on port `5000`
- So targetPort must be `5000` (not `8080`)

---

## ✅ Fix Steps

1. **Fix the deployment image**

Edit the deployment and set the correct image name:

```bash
kubectl edit deployment python-deployment-nautilus
```
Then change this line:

```yaml
image: poroko/flask-app-demo
```
to:

```yaml
image: poroko/flask-demo-app
```
Save and exit (`:wq`).

Or apply a direct patch:

```bash
kubectl set image deployment/python-deployment-nautilus python-container-nautilus=poroko flask-demo-app
```

2. **Fix the service ports**

Edit the service:

```bash
kubectl edit svc python-service-nautilus
```
Make sure it looks like this:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: python-service-nautilus
spec:
  type: NodePort
  selector:
    app: python_app
  ports:
    - port: 5000
      targetPort: 5000
      nodePort: 32345
```
Save and exit.

---

3. **Verify everything**

Check pods and service:

```bash
kubectl get pods -o wide
kubectl get svc python-service-nautilus
```
You should see something like:

```bash
NAME                               READY   STATUS    RESTARTS   AGE
python-deployment-nautilus-xxxxx   1/1     Running   0          1m
```
```bash
NAME                      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
python-service-nautilus   NodePort   10.96.203.251   <none>        5000:32345/TCP   10m
```
---

4. **Test the app**

From your jump_host, test using curl:

```bash
curl http://<node-ip>:32345
```
If it returns a Flask welcome message → ✅ success!