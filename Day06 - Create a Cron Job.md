# Day 6: Create a Cron Job

## Question

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

**Task:**  
a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.

b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

---

## Solution

```bash
ssh tony@stapp01
sudo -i
yum install -y cronie # For RedHat/CentOS based systems
apt-get install -y cron # For Debian/Ubuntu based systems
systemctl enable crond && systemctl start crond
crontab -e
```
Then add the cron job using:

```text
*/5 * * * * echo hello > /tmp/cron_text
```
---

## Verification

To list the current user's cron jobs:

```bash
crontab -l
```
Check service status:

```bash
sudo systemctl status cron.service
or
sudo systemctl status crond.service
```

**Example Output:**

```bash
[root@stapp01 ~]# systemctl enable crond && systemctl start crond
[root@stapp01 ~]# crontab -l
no crontab for root
[root@stapp01 ~]# crontab -e
[root@stapp01 ~]# crontab -l
*/5 * * * * echo hello > /tmp/cron_text
```
