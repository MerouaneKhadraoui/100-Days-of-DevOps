# Day 39: Create a Docker Image From Container

## Question

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:

**Task:**

a. Create an image `games:devops` on `Application Server 1` from a container `ubuntu_latest` that is running on same server.

---

## Solution

1. **SSH into the server**

```bash
ssh tony@stapp01
# password: Ir0nM@n
```

---

2. **Check running containers**

```bash
docker ps
```
Look for the container named `ubuntu_latest`.

---

3. **Commit the container into an image**

```bash
docker commit ubuntu_latest games:devops
```
- `ubuntu_latest` → the container name
- `blog` → image name
- `nautilus` → image tag

---

4. **Verify that the new image exists**

```bash
docker images
```
You should see something like:

```bash
REPOSITORY   TAG        IMAGE ID       CREATED          SIZE
blog         nautilus   abc123def456   few seconds ago  72MB
```
---

🚀 **Final Answer (single line command)**

```bash
docker commit ubuntu_latest games:devops
```
