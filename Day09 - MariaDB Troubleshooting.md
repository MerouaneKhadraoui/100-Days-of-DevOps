# Day 9: MariaDB Troubleshooting

## Question

There is a critical issue going on with the `Nautilus` application in `Stratos DC`. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.

**Task:**  

Look into the issue and fix the same.

---

## Solution

1. Initial Service Status Check:
Command: systemctl status mariadb or service mariadb status
Purpose: To determine if the MariaDB service is running, stopped, or in a failed state, and to view recent log entries for clues.

```bash
systemctl status mariadb
service mariadb status
```

2. Checking MariaDB Logs:
Location: Typically /var/log/mariadb/mariadb.log or /var/log/mysql/error.log

```bash
tail -f /var/log/mariadb/mariadb.log #to follow the log in real-time, or
cat /var/log/mariadb/mariadb.log | tail -n 50 #to view the last 50 lines.
```

Purpose: Logs provide detailed error messages and warnings that pinpoint the root cause of the issue.

3. Common Troubleshooting Scenarios:
- Permission Issues:
  - Symptom: "Permission denied" errors in logs, especially when MariaDB tries to write to its data directory (/var/lib/mysql) or PID file directory (/run/mariadb).
  - Resolution: Correct ownership and permissions using chown and chmod. For instance:

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chown -R mysql:mysql /run/mariadb
sudo chmod 755 /run/mariadb
```

---

## Verification

Check version:

```bash
systemctl start mariadb
systemctl status mariadb
```

**Example Output:**

```bash
thor@jumphost ~$ peter@stdb01
[peter@stdb01 ~]$ sudo -i
[root@stdb01 ~]# systemctl start mariadb
Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xeu mariadb.service" for details.
[root@stdb01 ~]# cat /var/log/mariadb/mariadb.log
[ERROR] InnoDB: The error means mysqld does not have the access rights to the directory.
[root@stdb01 ~]# ls -ld /var/lib/mysql/
drwxr-xr-x 1 root mysql 4096 Aug 12 17:39 /var/lib/mysql/
[root@stdb01 ~]# chown -R mysql:mysql /var/lib/mysql/
[root@stdb01 ~]# ls -ld /var/lib/mysql/
drwxr-xr-x 1 mysql mysql 4096 Aug 12 17:39 /var/lib/mysql/
[root@stdb01 ~]# systemctl start mariadb
[root@stdb01 ~]# systemctl status mariadb
mariadb.service - MariaDB 10.5 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; preset: disabled)
     Active: active (running) since Tue 2025-08-12 18:10:35 UTC; 26s ago
```

