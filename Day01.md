# Day 1: Linux User Setup with Non-Interactive Shell

## Question

To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell.

**Task:**  
Create a user named `javed` with a non-interactive shell on `App Server 2`.

---

## Solution

```bash
ssh banner@stapp02
sudo useradd -s /sbin/nologin javed
```
Type the password for the user when prompted to complete the user creation process.

---

## Verification

To verify that the user has been created with a non-interactive shell, check the user's shell by running:

```bash
getent passwd javed
```

If the user has been created successfully, the output should show `/sbin/nologin` as the shell for the user `javed`.

**Example Output:**

```text
[root@stapp02 ~]# getent passwd javed
javed:x:1002:1002::/home/javed:/sbin/nologin
```
