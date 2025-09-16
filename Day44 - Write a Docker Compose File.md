# Day 44: Write a Docker Compose File

## Question

The Nautilus application development team shared static website content that needs to be hosted on the `httpd` web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:

**Task:**

a. On `App Server 2` in `Stratos DC` create a container named `httpd` using a docker compose file `/opt/docker/docker-compose.yml` (please use the exact name for file).

b. Use `httpd` (preferably `latest` tag) image for container and make sure container is named as `httpd`; you can use any name for service.

c. Map `80` number port of container with port `8085` of docker host.

d. Map container's `/usr/local/apache2/htdocs` volume with `/opt/devops` volume of docker host which is already there. (please do not modify any data within these locations).

---

## ✅ Solution Dockerfile

Here’s the correct solution:

```yaml
version: '3'
services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8085:80"
    volumes:
      - /opt/devops:/usr/local/apache2/htdocs
```

---

## 🔎 Explanation

1. **Create the directory if not exists**

```bash
sudo mkdir -p /opt/docker
```

2. **Create the docker-compose file**

```bash
sudo vi /opt/docker/docker-compose.yml
```
(Paste the above YAML content inside)

3. **Run the container**

```bash
cd /opt/docker
sudo docker-compose up -d
```

4. **Verify**

```bash
sudo docker ps
```
You should see a container named httpd running, port `8085` mapped, and the volume mounted.

---

✅ This matches exactly the task requirements.