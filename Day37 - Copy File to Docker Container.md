# Day 37: Copy File to Docker Container

## Question

The Nautilus DevOps team possesses confidential data on `App Server 2` in the `Stratos Datacenter`. A container named `ubuntu_latest` is running on the same server.

**Task:**

Copy an encrypted file `/tmp/nautilus.txt.gpg` from the docker host to the `ubuntu_latest` container located at `/tmp/`. Ensure the file is not modified during this operation.

---

## Solution

1. **Verify the container is running**

```bash
docker ps | grep ubuntu_latest
```

---

2. **Copy the file into the container**

```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

---

3. **Confirm inside the container**

```bash
docker exec -it ubuntu_latest ls -l /tmp/
```
You should see:

```bash
-rw-r--r--  1 root root     nautilus.txt.gpg
```
---

✅ That’s it. The file is copied as-is without modification.