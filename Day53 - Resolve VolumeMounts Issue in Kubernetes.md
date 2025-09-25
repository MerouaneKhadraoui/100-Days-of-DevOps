# Day 53: Resolve VolumeMounts Issue in Kubernetes

## Question

We encountered an issue with our Nginx and PHP-FPM setup on the Kubernetes cluster this morning, which halted its functionality. Investigate and rectify the issue:

**Task:**

The pod name is `nginx-phpfpm` and configmap name is `nginx-config`. Identify and fix the problem.

Once resolved, copy `/home/thor/index.php` file from the `jump host` to the `nginx-container` within the nginx document root. After this, you should be able to access the `website` using Website button on the top bar.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Inspect the Pod definition**

```bash
kubectl describe pod nginx-phpfpm
kubectl get pod nginx-phpfpm -o yaml
```
Check for `volumes:` and `volumeMounts:` — look for mismatches:
- ConfigMap name should be `nginx-config`.
- It should mount to `/var/www/html`.

---

2. **Fix the Deployment/Pod Spec**

If misconfigured, edit:

```bash
kubectl edit pod nginx-phpfpm
```
(or if it’s a Deployment: `kubectl edit deploy nginx-phpfpm`).

Correct `volumeMounts`:

```yaml
volumeMounts:
  - name: shared-files
    mountPath: /var/www/html
```
👉 This ensures the `nginx-config` ConfigMap is mounted correctly.

---

3. **Restart the Pod**

If edited directly:

```bash
kubectl delete pod nginx-phpfpm
```
(It will be recreated if controlled by Deployment.)

Check status:

```bash
kubectl get pods
```
---

4. **Copy index.php into container**

On jump_host:

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/ -c nginx-container
```
---

5. **Verify**

- Website button (top bar in KodeKloud) should now load the PHP page.

---

## ✅ Final Answer (short summary you’d give in KodeKloud):

- Fixed the `VolumeMounts` by properly mounting `nginx-config` ConfigMap at `/var/www/html` using subPath.
- Deleted and restarted the `nginx-phpfpm` pod.
- Copied `/home/thor/index.php` into `/var/www/html` of `nginx-container`.
- Verified website is accessible.