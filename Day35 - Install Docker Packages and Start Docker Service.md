# Day 35: Install Docker Packages and Start Docker Service

## Question

The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:

**Task:**

1. Install `docker-ce` and `docker compose` packages on `App Server 2`.

2. Initiate the `docker` service.

---

## Solution


## Step 1 - SSH into the server

```bash
ssh steve@stapp02
# password: Am3ric@
```

---

## Step 2 - Install prerequisites

Before installing Docker, we need to remove any old versions and install the required dependencies:

```bash
sudo yum update -y
sudo yum remove docker docker-client docker-client-latest docker-common \
docker-latest docker-latest-logrotate docker-logrotate docker-engine -y

sudo yum install -y yum-utils
```

---

## Step 3 - Add the Docker repository

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

---

## Step 4 - Install Docker CE and Docker Compose

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

---

**Note:**
Newer versions of Docker include `docker compose` as a plugin (`docker compose` instead of `docker-compose`).

---

## Step 5 - Start and enable Docker service

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## Step 6 - Verify installation

```bash
docker --version
docker compose version   # or docker-compose --version if using old binary
sudo docker run hello-world
```

✅ **At this point:**

- docker-ce is installed.
- docker compose is available.
- The Docker service is running and enabled at boot.