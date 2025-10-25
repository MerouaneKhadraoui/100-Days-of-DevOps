# Day 73: Jenkins Scheduled Jobs

## Question

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:

**Task:**

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`

1. Create a Jenkins jobs named `copy-logs`.

2. Configure it to periodically build `every 12 minutes` to copy the Apache logs (`both access_log and error_logs`) from `App Server 3` (`from default logs location`) to location `/usr/src/dba` on `Storage Server`.

`Note:` 

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

---

# Day 73: Jenkins Scheduled Jobs — Solution

## 🎯 Goal
Create a Jenkins job named `copy-logs` that runs **every 12 minutes** and copies Apache logs (`access_log` and `error_log`) from **App Server 3 (stapp03)** to **/usr/src/dba** on **Storage Server (ststor01)**.

---

## ⚙️ Step 1: Install the Plugin
1. Go to **Manage Jenkins → Manage Plugins → Available**.
2. Search for **Publish Over SSH**.
3. Install it and check ✅ *Restart Jenkins when installation is complete and no jobs are running*.
4. Refresh the UI after restart.

---

## 🔐 Step 2: Configure SSH Connections
1. Navigate to **Manage Jenkins → Configure System**.
2. Scroll to **Publish over SSH** section.
3. Add two servers:

### App Server 3
```
Name: stapp03
Hostname: stapp03
Username: banner
```

### Storage Server
```
Name: ststor01
Hostname: ststor01
Username: natasha
```

4. Click **Test Configuration** for both and ensure connections succeed.
5. Click **Save**.

---

## 🧱 Step 3: Create Jenkins Job
1. Dashboard → **New Item**
2. Enter name: `copy-logs`
3. Select **Freestyle project** → click **OK**

---

## ⏱️ Step 4: Schedule Job (Every 12 Minutes)
Go to **Build Triggers** and enable ✅ **Build periodically**.

Cron expression:
```
*/12 * * * *
```

---

## 🧩 Step 5: Build Steps Using Publish Over SSH

**Add build step:** *Send files or execute commands over SSH*

- **SSH Server:** `stapp03`
- **Exec command:**
  ```bash
  scp /var/log/httpd/*log natasha@ststor01:/usr/src/dba/
  ```
---

## 💾 Step 6: Save and Run
Click **Save**, then **Build Now** to test manually.

Expected console output:
```
[SSH] executing command...
sshpass -p Bl@kW scp /var/log/httpd/*log natasha@ststor01:/usr/src/dba/
[SSH] command execution finished
```

---

## 🧾 Step 7: Verify on Storage Server
SSH into the storage server:
```bash
ssh natasha@ststor01
ls -l /usr/src/dba
```

Expected result:
```
access_log
error_log
```

---

## ✅ Summary

| Step | Description | Value |
|------|--------------|--------|
| Job Name | `copy-logs` | ✅ |
| Plugin Used | Publish Over SSH | ✅ |
| Schedule | `*/12 * * * *` | Every 12 min |
| Source Server | `stapp03` (App Server 3) | ✅ |
| Destination Server | `ststor01` (Storage Server) | ✅ |
| Destination Path | `/usr/src/dba` | ✅ |

---

## 🧠 Tip
If SSH key verification causes issues, you can add this global environment variable in Jenkins:

```
Key: GIT_SSH_COMMAND
Value: ssh -o StrictHostKeyChecking=no
```

---

**✅ Congratulations! You’ve successfully completed Day 73 — Jenkins Scheduled Jobs using the Publish Over SSH plugin.**