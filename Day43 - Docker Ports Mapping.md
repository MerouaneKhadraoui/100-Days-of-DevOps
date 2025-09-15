# Day 43: Docker Ports Mapping

## Question

The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on `Application Server 3` in `Stratos Datacenter`. Please perform the task as per details mentioned below:

**Task:**

a. Pull `nginx:alpine` docker image on `Application Server 3`.

b. Create a container named `cluster` using the image you pulled.

c. Map host port `8084` to container port `80`. Please keep the container in running state.

---

## Solution

1. **SSH into App Server 3**

```bash
ssh banner@stapp03
# password: BigGr33n
```

---

2. **Pull the image**

```bash
docker pull nginx:alpine
```

---

3. **Run a container with port mapping**

Map host port `8084` → container port `80`:

```bash
docker run -d --name cluster -p 8084:80 nginx:alpine
```

- `-d` → run in detached (background) mode.
- `--name cluster` → container name.
- `-p 8084:80` → map host port `8084` to container port `80`.

---

4. **Verify container is running**

Run: 

```bash
docker ps
```
You should see something like:

```nginx
CONTAINER ID   IMAGE          COMMAND                  PORTS                  NAMES
abc12345       nginx:alpine   "nginx -g 'daemon of…"   0.0.0.0:8084->80/tcp   cluster
```
Make sure:

---

5. **(Optional) Test nginx page**

From Application Server 3:

```bash
curl http://localhost:8084
```
You should get the default nginx welcome page HTML.

---

🔑 Final Command (if you want it in one go):

```bash
docker pull nginx:alpine && docker run -d --name cluster -p 8084:80 nginx:alpine
```