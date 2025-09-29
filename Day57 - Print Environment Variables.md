# Day 57: Print Environment Variables

## Question

The Nautilus DevOps team is working on to setup some pre-requisites for an application that will send the greetings to different users. There is a sample deployment, that needs to be tested. Below is a scenario which needs to be configured on Kubernetes cluster. Please find below more details about it.

**Task:**

1. Create a `pod` named `print-envars-greeting`.

2. Configure spec as, the container name should be `print-env-container` and use `bash` image.

3. Create three environment variables:

  a. `GREETING` and its value should be `Welcome to`

  b. `COMPANY` and its value should be `Stratos`

  c. `GROUP` and its value should be `Industries`

4. Use command `["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']` (please use this exact command), also set its `restartPolicy` policy to `Never` to avoid crash loop back.

5. You can check the output using `kubectl logs -f print-envars-greeting` command.

`Note:` The `kubectl` utility on `jump_host` is configured to operate with the Kubernetes cluster.

---

## Solution

1. **Create the YAML file**

Run this on the `jump_host`:

```bash
vi print-envars-greeting.yaml
```

Paste the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
  - name: print-env-container
    image: bash
    command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
    env:
    - name: GREETING
      value: "Welcome to"
    - name: COMPANY
      value: "Stratos"
    - name: GROUP
      value: "Industries"
```

---

2. **Apply it**

```bash
kubectl apply -f print-envars-greeting.yaml
```

---

3. **Verify pod status**

```bash
kubectl get pods
```
It should go into **Completed** state (since it just runs echo once).

---

4. **Check logs**

```bash
kubectl logs -f print-envars-greeting
```

✅ Expected Output:

```css
Welcome to Stratos Industries
```

---

👉 This manifest meets all requirements:

- **Name**: `nginx-replicaset`
- **Container name**: `nginx-container`
- **Image**: `nginx:latest`
- **Replicas**: 4
- **Labels**: `app=nginx_app`, `type=front-end`