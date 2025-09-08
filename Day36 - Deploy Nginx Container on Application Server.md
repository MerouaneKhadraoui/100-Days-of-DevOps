# Day 36: Deploy Nginx Container on Application Server

## Question

The Nautilus DevOps team is conducting application deployment tests on selected application servers. They require a nginx container deployment on `Application Server 1`. Complete the task with the following instructions:

**Task:**

On `Application Server 1` create a container named `nginx_1` using the `nginx` image with the `alpine` tag. Ensure container is in a `running` state.

---

## Solution

1. **SSH into the server**

```bash
ssh tony@stapp01
# password: Ir0nM@n
```

---

2. **Pull the nginx alpine image (optional, Docker will auto-pull if not found)**

```bash
docker pull nginx:alpine
```

---

3. **Run the container with the required name**

```bash
docker run -d --name nginx_1 nginx:alpine
```
- `-d` → runs container in background (detached mode)
- `--name nginx_1` → names the container
- `nginx:alpine` → uses the nginx image with the `alpine` tag

---

4. **Verify the container is running**

```bash
docker ps | grep nginx_1
```
You should see the container with status `Up`.

---

🔑 **Final Answer**

On Application Server 1, run:

```bash
docker run -d --name nginx_1 nginx:alpine
```