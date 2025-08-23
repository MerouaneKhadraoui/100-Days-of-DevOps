# Day 19: Install and Configure Web Application

## Question

xFusionCorp Industries is planning to host two static websites on their infra in `Stratos Datacenter`. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:

**Task:**  

a. Install `httpd` package and dependencies on `app server 2`.

b. Apache should serve on port `6400`.

c. There are two website's backups `/home/thor/ecommerce` and `/home/thor/cluster` on `jump_host`. Set them up on Apache in a way that `ecommerce` should work on the link `http://localhost:6400/ecommerce/` and `cluster` should work on link `http://localhost:6400/cluster/` on the mentioned app server.

d. Once configured you should be able to access the website using `curl` command on the respective app server, i.e `curl http://localhost:6400/ecommerce/` and `curl http://localhost:6400/cluster/`

---

## Solution

1. **Connect to App Server 2**

    From the jump host, use SSH:

```bash
ssh steve@stapp02
```

---

2. **Install Apache (httpd)**

```bash
sudo yum install -y httpd
```

---

3. **Configure Apache to Listen on Port 6400**

```bash
cat /etc/httpd/conf/httpd.conf | grep Listen
#Listen 80
sudo sed -i 's/^Listen 80/Listen 6400/' /etc/httpd/conf/httpd.conf
cat /etc/httpd/conf/httpd.conf | grep Listen
#Listen 6400
```

---

4. **Copy Website Files from Jump Host**

On the jump host, the backups are in /home/thor/.

Copy them to App Server 2:

```bash
scp -r /home/thor/ecommerce /home/thor/cluster tony@stapp02:/tmp/
```

---

5. **Move Website Files to Apache Web Root**

On App Server 2:

```bash
sudo mv /tmp/ecommerce /var/www/html/
sudo mv /tmp/cluster /var/www/html/
```
Now you should have:

```bash
/var/www/html/ecommerce
/var/www/html/cluster
```

---

6. **Create Alias Configuration (Optionnal)**

Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Add at the end:

```apache
Alias /ecommerce /var/www/html/ecommerce
<Directory /var/www/html/ecommerce>
    AllowOverride None
    Require all granted
</Directory>

Alias /cluster /var/www/html/cluster
<Directory /var/www/html/cluster>
    AllowOverride None
    Require all granted
</Directory>
```

---

7. **Start & Enable Apache**

Edit Apache config:

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

8. **Test with curl**

On App Server 2:

```bash
curl http://localhost:6400/ecommerce/
curl http://localhost:6400/cluster/
```

You should see the respective website content.

✅ Done! Now both sites are accessible under the required paths.