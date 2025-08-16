# Day 12: Linux Network Services

## Question

Our monitoring tool has reported an issue in `Stratos Datacenter`. One of our app servers has an issue, as its Apache service is not reachable on port `8087` (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

**Task:**  

- Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

- Once fixed, you can test the same using command `curl http://stapp01:8087` command from jump host.

---

## Solution

1. **Step 1: SSH into the app server**

From the jump host:

```bash
ssh tony@stapp01
# Password : Ir0nM@n
```

---

2. **Step 2: Check if Apache is running**

```bash
sudo systemctl status httpd
```

If it's not running:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

But if it's already running, move to the next check.

---

3. **Step 3: Check if Apache is listening on port 8087**

```bash
sudo netstat -tulpn | grep 8087
# OR
sudo ss -tulpn | grep 8087
```

You'll see something like:

```nginx
tcp   LISTEN 0 128 0.0.0.0:8087   0.0.0.0:*   users:(("sendm",pid=443,fd=4))
```

If sendm is not needed: stop it.

```bash
sudo systemctl stop sendm
sudo systemctl disable sendm
# OR
sudo kill 443
```

If it's not listening on 8087, you need to check Apache's config.

---

4. **Step 4: Verify Apache configuration for port 8087**

Open the main config file:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```
Look for the `Listen` directive:

```apache
Listen 8087
```
If it’s missing or wrong (e.g., says `Listen 80`), change it to:

```apache
Listen 8087
```
Save and restart Apache:

```bash
sudo systemctl restart httpd
```
---

5. **Step 5: Check firewall or iptables rules**

```bash
sudo firewall-cmd --list-all
```

If `8087/tcp` is missing:

```bash
sudo firewall-cmd --permanent --add-port=8087/tcp
sudo firewall-cmd --reload
```

OR

```bash
sudo iptables -L -n
```
If 8087 is blocked or not present, allow it:

```bash
sudo iptables -I INPUT -p tcp --dport 8087 -j ACCEPT

sudo service iptables save
sudo service iptables restart
# or
sudo iptables-save | sudo tee /etc/sysconfig/iptables
sudo systemctl restart iptables
```

---

6. **Step 6: Check SELinux (if enabled)**

If SELinux is enforcing, it might block non-standard ports.

```bash
sudo getenforce
```

If `Enforcing`:

```bash
sudo semanage port -l | grep http_port_t
```
If `8087` is missing:

```bash
sudo semanage port -a -t http_port_t -p tcp 8087
```
Then restart Apache:

```bash
sudo systemctl restart httpd
```

---

7. **Step 7: Test from jump host**

Exit from app server:

```bash
exit
```

From the jump host:

```bash
curl http://stapp01:8087
```
If you see HTML output or the Apache default page, the issue is fixed.

---
