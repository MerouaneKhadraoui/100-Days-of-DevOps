# Day 42: Create a Docker Network

## Question

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:

**Task:**

a. Create a docker network named as `news` on `App Server 2` in `Stratos DC`.

b. Configure it to use `bridge` drivers.

c. Set it to use subnet `192.168.30.0/24` and iprange `192.168.30.0/24`.

---

## Solution

1. **SSH into App Server 2**

```bash
ssh steve@stapp02
# password: Am3ric@
```

---

2. **Create the Docker network**

Use the `docker network create` command with the required options:

```bash
docker network create \
  --driver=bridge \
  --subnet=192.168.30.0/24 \
  --ip-range=192.168.30.0/24 \
  news

  #or 

docker network create --driver bridge --subnet 192.168.30.0/24 --ip-range 192.168.30.0/24 news 
```

---

3. **Verify the network**

Run: 

```bash
docker network ls
```
You should see a line with `news` listed.

Then check its details:

```bash
docker network inspect news
```
Make sure:

- **Driver** is `bridge`
- **Subnet** is `192.168.30.0/24`
- **IPRange** is `192.168.30.0/24`

---

✅ That completes the task.