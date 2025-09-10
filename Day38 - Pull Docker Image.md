# Task 1: Install Docker Packages and Start Docker Service

## Question

Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:

**Task:**

a. Pull `busybox:musl` image on `App Server 1` in `Stratos DC` and re-tag (create new tag) this image as `busybox:news`.

---

## Solution


## Step 1 - SSH into the server

```bash
ssh tony@stapp01
# password: Ir0nM@n
```

---

## Step 2 - Pull the required image


```bash
docker pull busybox:musl
```

---

## Step 3 - Re-tag the image

```bash
docker tag busybox:musl busybox:news
```

---

## Step 4 - Verify both tags exist

```bash
docker images | grep busybox
```
✅ You should see both:

```bash
busybox   musl   <IMAGE_ID>   ...
busybox   news   <IMAGE_ID>   ...
```
