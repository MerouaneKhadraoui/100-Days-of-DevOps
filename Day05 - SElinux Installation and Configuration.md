# Day 5: SElinux Installation and Configuration

## Question

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 1` in the `Stratos Datacenter`:

**Task:**  
1. Install the required `SELinux` packages.

2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.

3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.

4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.

---

## Solution

1. **Login to App Server 1 & Switch to root**

   ```bash
   ssh tony@stapp01
   sudo -i
   ```

2. **Installed the Packages**

   Run the following command to install the required SELinux packages:

   ```bash
   yum install -y selinux-policy selinux-policy-targeted
   ```

3. **Edit the SELinux configuration file**

   ```bash
   vi /etc/selinux/config
   ```
   
   Then changed SELINUX from "enforcing to disabled"

   ```bash
   SELINUX=disabled
   ```

---

## Verification

To verify SELinux Status:

```bash
sestatus
getenforce
```

**Expected Output:**

```bash
[root@stapp01 ~]# sestatus
SELinux status: disabled
[root@stapp01 ~]# getenforce
Disabled
```

---

## Note

No reboot needed because the reboot is already scheduled in the task.