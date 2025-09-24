# Day 52: Revert Deployment to Previous Version in Kubernetes

## Question

Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.

**Task:**

There exists a deployment named `nginx-deployment`; initiate a rollback to the previous revision.

`Note:` The `kubectl` utility on `jump_host` is set up to interact with the Kubernetes cluster.

---

## Solution

1. **Check rollout history of the deployment**

```bash
kubectl rollout history deployment nginx-deployment
```
👉 This will show you the revisions (e.g., Revision 1, Revision 2, etc.).

---

2. **Rollback to the previous revision**

```bash
kubectl rollout undo deployment nginx-deployment
```
👉 By default, this reverts to the **immediately previous revision**.

---

3. **(Optional) Rollback to a specific revision**

If they explicitly want you to revert to a known revision (e.g., `1`), use:

```bash
kubectl rollout undo deployment nginx-deployment --to-revision=1
```
---

4. **Verify the rollback status**

```bash
kubectl rollout status deployment nginx-deployment
```
You should see output like:

```csharp
deployment "nginx-deployment" successfully rolled out
```
---

5. **Confirm the rollback worked by checking the image/version in the deployment**

```bash
kubectl describe deployment nginx-deployment | grep Image
```
---

## 🔎✅ Final Answer (short & practical for KodeKloud)

```bash
kubectl rollout undo deployment nginx-deployment
kubectl rollout status deployment nginx-deployment
```