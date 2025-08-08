# Day 1: Linux User Setup with Non-Interactive Shell

## Question

To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell.

**Task:**  
Create a user named `ravi` with a non-interactive shell on App Server 3.

---

## Solution

```bash
ssh banner@stapp03
sudo useradd -s /sbin/nologin ravi
```
Type the password for the user when prompted to complete the user creation process.

---

## Verification

To verify that the user has been created with a non-interactive shell, check the user's shell by running:

```bash
getent passwd ravi
```

If the user has been created successfully, the output should show `/sbin/nologin` as the shell for the user `ravi`.

**Example Output:**

```text
[root@stapp03 ~]# getent passwd ravi
ravi:x:1002:1002::/home/ravi:/sbin/nologin
```
