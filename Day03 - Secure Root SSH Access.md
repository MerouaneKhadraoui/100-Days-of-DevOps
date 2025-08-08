# Day 3: Secure Root SSH Access

## Question

Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.

**Task:**  
Disable direct SSH root login on all app servers within the `Stratos Datacenter`.

---

## Solution

1. **Edit the SSH Configuration File**

   ```bash
   sudo vi /etc/ssh/sshd_config
   ```

   Locate the line:

   ```text
   #PermitRootLogin yes
   ```

   Change it to:

   ```text
   PermitRootLogin no
   ```

   If the line is commented (`#`), remove the `#` and set the value to `no`.

2. **Restart the SSH Service**

   After saving the file, restart the SSH daemon to apply changes:

   ```bash
   sudo systemctl restart sshd
   ```
   On some systems, the service may be named `ssh` instead of `sshd`.

---

## Verification

To verify the change, run:

```bash
sudo grep PermitRootLogin /etc/ssh/sshd_config
```

**Expected Output:**

```text
PermitRootLogin no
```