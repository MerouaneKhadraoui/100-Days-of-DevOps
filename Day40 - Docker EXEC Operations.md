# Day 40: Docker EXEC Operations

## Question

One of the Nautilus DevOps team members was working to configure services on a `kkloud`  container that is running on `App Server 3` in `Stratos Datacenter`. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:

**Task:**

a. Install `apache2` in `kkloud` container using `apt` that is running on `App Server 3` in `Stratos Datacenter`.

b. Configure Apache to listen on port `8085` instead of default `http` port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

---

## Solution

1. **SSH into the server**

```bash
ssh banner@stapp03
# password: BigGr33n
```

---

2. **Check the container running**

```bash
docker ps
```
You should see a container named `kkloud`.

---

3. **Access the container**

```bash
docker exec -it kkloud bash
```

---

4. **Update packages and install apache2**

Inside the container:

```bash
apt-get update
apt-get install -y apache2
```

---

5. **Change Apache port to 8085**

Open Apache ports config:

```bash
grep 'Listen 80' /etc/apache2/ports.conf
vi /etc/apache2/ports.conf
#Find Listen 80 and replace by Listen 8085, or
sed -i  's/Listen 80/Listen 8085/g' /etc/apache2/ports.conf
```

Then update the default site config:

```bash
grep '*:80' /etc/apache2/sites-available/000-default.conf
vi /etc/apache2/sites-available/000-default.conf
#Find <VirtualHost *:80> and replace by <VirtualHost *:8085>, or
sed -i  's/80/8085/g' /etc/apache2/sites-available/000-default.conf
```

---

6. **Restart Apache service**

```bash
service apache2 restart
```

---

7. **Verify Apache is running on port 8085**

```bash
netstat -tuln | grep 8085
# or
ss -tuln | grep 8085
```
You should see it listening.

---

8. **Exit the container but keep it running**

Just type:

```bash
exit
```
⚠️ Do not stop or kill the container — it should stay running with Apache serving on port 8085.

---

✅ That’s it! Task is complete:

- Apache installed in kkloud
- Configured on port 8085
- Service running
- Container kept running