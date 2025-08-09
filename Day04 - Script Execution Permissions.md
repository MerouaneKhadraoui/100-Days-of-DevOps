# Day 4: Script Execution Permissions

## Question

In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.

**Task:**  
Grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 1`. Ensure that all users have the capability to execute it.

---

## Solution

1. **Login to App Server 1**

   ```bash
   ssh tonay@stapp01
   ```

2. **Set Permissions**

   Run the following command to make the script readable and executable by all users:

   ```bash
   sudo chmod 755 /tmp/xfusioncorp.sh
   ```

   This sets:
   - **Owner:** read, write, execute (`7`)
   - **Group:** read, execute (`5`)
   - **Others:** read, execute (`5`)

---

## Verification

To verify the permissions, run:

```bash
ls -l /tmp/xfusioncorp.sh
```

**Expected Output:**

```bash
-rwxr-xr-x 1 root root 40 Aug  7 17:49 /tmp/xfusioncorp.sh
```

---

## Note

If the file has no permissions at all (`----------`), using `chmod +x` alone may not work unless the file is at least readable.  
Using `chmod 755` explicitly sets all necessary permission bits.