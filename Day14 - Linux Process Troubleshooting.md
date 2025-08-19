# Day 14: Linux Process Troubleshooting

## Question

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in `Stratos DC`.

**Task:**  

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don't need to worry if Apache isn't serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port `8085` on all app servers.

---

## Solution

1. **Install `iptables`**

On each app host (e.g., app1, app2, app3 in Nautilus labs):

```bash
sudo yum install -y iptables iptables-services   # for CentOS/RHEL
# or
sudo apt-get update && sudo apt-get install -y iptables-persistent   # for Ubuntu/Debian
```

---

2. **Block Apache port `3003` for all except the LBR host**

Let's say the LBR host IP is 172.16.238.14 (replace with the actual IP in the lab).

Run on each app host:

```bash
# Flush old rules (clean start)
sudo iptables -F

# Allow loopback traffic
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH (important so you don't lock yourself out)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow Apache port 3003 ONLY from LBR host
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 3003 -j ACCEPT

# Drop all other traffic to port 3003
sudo iptables -A INPUT -p tcp --dport 3003 -j DROP

# (Optional) Drop everything else not explicitly allowed
# sudo iptables -A INPUT -j DROP
```

---

3. **Make Rules Persistent After Reboot**

***CentOS/RHEL:***

```bash
# Save rules
sudo service iptables save

# Enable service at boot
sudo systemctl enable iptables
sudo systemctl start iptables
```

***Ubuntu/Debian:***

During install of iptables-persistent, it asks to save rules.
If not, you can do:

```bash
sudo netfilter-persistent save
sudo systemctl enable netfilter-persistent
```

---

4. **Verify**

From the **LBR host:**

```bash
curl http://stapp01:3003   # should work
curl http://stapp02:3003   # should work
curl http://stapp03:3003   # should work
```

From any other host (e.g., jump host):

```bash
curl http://stapp01:3003   # should FAIL
```
---
