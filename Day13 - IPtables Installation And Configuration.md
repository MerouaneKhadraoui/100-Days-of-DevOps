# Day 13: IPtables Installation And Configuration

## Question

We have one of our websites up and running on our `Nautilus` infrastructure in `Stratos DC`. Our security team has raised a concern that right now Apache's port i.e `3003` is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:

**Task:**  

1. Install `iptables` and all its dependencies on each app host.

2. Block incoming port `3003` on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.

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

From the *LBR host:*

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
