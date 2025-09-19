# Day 46: Deploy an App on Docker Containers

## Question

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:

**Task:**

1. On `App Server 3` in `Stratos Datacenter` create a docker compose file `/opt/devops/docker-compose.yml` (should be named exactly).

2. The compose should deploy two services (web and DB), and each service should deploy a container as per details below:

`For web service:`

a. Container name must be `php_apache`.

b. Use image `php` with any `apache` tag. Check here for more details.

c. Map `php_apache` container's port `80` with host port `3002`

d. Map `php_apache` container's `/var/www/html` volume with host volume `/var/www/html`.

`For DB service:`

a. Container name must be `mysql_apache`.

b. Use image `mariadb` with any tag (preferably `latest`). Check here for more details.

c. Map `mysql_apache` container's port `3306` with host port `3306`

d. Map `mysql_apache` container's `/var/lib/mysql` volume with host volume `/var/lib/mysql`.

e. Set MYSQL_DATABASE=`database_apache` and use any custom user ( except root ) with some complex password for DB connections.

3. After running docker-compose up you can access the app with curl command `curl <server-ip or hostname>:3002/`

`Note:` Once you click on `FINISH` button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

---

## Solution

1. **Create the docker-compose.yml file**

Path required:
`/opt/devops/docker-compose.yml`

```yaml
services:
  web:
    container_name: php_apache
    image: php:apache
    ports:
      - "3002:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_apache
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=RootPass123!
      - MYSQL_DATABASE=database_apache
      - MYSQL_USER=devuser
      - MYSQL_PASSWORD=AppUserPass!2025
```

---

2. **Steps to Deploy**

Run these commands on App Server 3:

```bash
# Create directory if not exists
sudo mkdir -p /opt/devops

# Create the docker-compose file
sudo vi /opt/devops/docker-compose.yml
# (paste the above YAML content)

# Start the stack in detached mode
cd /opt/devops
sudo docker compose up -d
```

3. **Verification**

Check containers are running:

```bash
docker ps
```
You should see `php_apache` and `mysql_apache` containers.

Test the web service:

```bash
curl http://localhost:3002/
```
(or use the server’s IP instead of localhost).

---

✅ With this, you’ll have:
- A `php_apache` container running on port `3002`.
- A `mysql_apache` container with database `database_apache`, user `devuser`, and a secure password.
- Proper volume mappings to persist data and code.