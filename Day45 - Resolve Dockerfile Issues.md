# Day 45: Resolve Dockerfile Issues

## Question

The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a `Dockerfile` on `App Server 1` in `Stratos DC`. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:

**Task:**

a. The `Dockerfile` is placed on `App Server 1` under `/opt/docker` directory.

b. Fix the issues with this file and make sure it is able to build the image.

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, index.html.


`Note:` Please note that once you click on `FINISH` button all the existing containers will be destroyed and new image will be built from your `Dockerfile`.

```Dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY /server.crt /usr/local/apache2/conf/server.crt

COPY /server.key /usr/local/apache2/conf/server.key

COPY ./index.html /usr/local/apache2/htdocs/
```

---

## Problems in the given `Dockerfile`

1. **Path to `httpd.conf`**

- In the base image `httpd:2.4.43`, the correct path is:
`/usr/local/apache2/conf/httpd.conf`

- But in the Dockerfile, you have mixed:
```Dockerfile
/usr/local/apache2/conf/httpd.conf
conf/httpd.conf
```
→ second one (conf/httpd.conf) will fail because Docker can’t find it.

---

2. **SSL certs copy**

- Currently:

```Dockerfile
COPY /server.crt /usr/local/apache2/conf/server.crt
COPY /server.key /usr/local/apache2/conf/server.key
```
- The leading `/` means **absolute path on build host**. Unless your files are literally at `/server.crt` on your build machine, this fails.

- They should be relative to the `Dockerfile` build context (`/opt/docker`).

---

3. **Index.html copy**

- This one looks fine, assuming `index.html` is in `/opt/docker`.

---

## Fixed `Dockerfile`

Here’s the corrected version ✅:

```Dockerfile
FROM httpd:2.4.43

# Change Apache port from 80 to 8080
RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

# Enable SSL-related modules and config
RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Copy SSL certificates (must exist in /opt/docker)
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key

# Copy custom index.html
COPY html/index.html /usr/local/apache2/htdocs/
```
---

## What to check before building

1. Make sure these files exist under `/opt/docker` on **App Server 1**:

- `Dockerfile`
- `server.crt`
- `server.key`
- `index.html`

You can confirm with:

```bash
ls -l /opt/docker
```
---

2. Then build the image:

```bash
cd /opt/docker
docker build -t myhttpd .
```
---

3. Run a container to test:

```bash
docker run -d -p 8080:8080 --name testhttpd myhttpd
```
Visit `http://localhost:8080` → should serve your `index.html`.