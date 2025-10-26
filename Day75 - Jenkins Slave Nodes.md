# Day 75: Jenkins Slave Nodes

## Question

The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.

**Task:**

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`

1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for `app server 1`, `app server 2` and `app server 3` must be `App_server_1`, `App_server_2`, `App_server_3` respectively.

2. Add labels as below:

`App_server_1 : stapp01`

`App_server_2 : stapp02`

`App_server_3 : stapp03`

3. Remote root directory for `App_server_1` must be `/home/tony/jenkins`, for `App_server_2` must be `/home/steve/jenkins` and for `App_server_3` must be `/home/banner/jenkins`.

4. Make sure slave nodes are online and working properly.

`Note:` 

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

---

### Solution

## Objective

Add all application servers as Jenkins slave nodes (SSH build agents) in Jenkins and ensure they are online.

---

## Environment Details

- **Jenkins Login:**
  - Username: `admin`
  - Password: `Adm!n321`

- **App Servers:**

| Server        | Hostname | User   | Remote Root Directory      |
|----------------|-----------|--------|-----------------------------|
| App Server 1   | stapp01   | tony   | /home/tony/jenkins         |
| App Server 2   | stapp02   | steve  | /home/steve/jenkins        |
| App Server 3   | stapp03   | banner | /home/banner/jenkins       |

---

## Steps to Configure Jenkins Slave Nodes

### Step 1: Access Jenkins
1. Click the **Jenkins** button in the KodeKloud lab.
2. Login using the provided credentials.

---

### Step 2: Add a New Node (App_server_1)
1. Navigate to **Manage Jenkins → Nodes → New Node**.
2. Enter node name: `App_server_1`
3. Choose **Permanent Agent** → click **OK**.
4. Fill in the following:
   - **Remote root directory:** `/home/tony/jenkins`
   - **Labels:** `stapp01`
   - **Launch method:** Launch agent via SSH
   - **Host:** `stapp01`
   - **Credentials:** Add new → SSH Username with private key
     - Username: `tony`
     - Private Key: paste the SSH private key from the control node
     - Save credential and select it.
   - Host Key Verification: choose **Non verifying Verification Strategy**
5. Save and apply.

---

### Step 3: Repeat for the Other Two Servers

| Node Name     | Label  | Hostname | User   | Remote Root Directory |
|----------------|--------|-----------|--------|------------------------|
| App_server_2   | stapp02 | stapp02   | steve  | /home/steve/jenkins   |
| App_server_3   | stapp03 | stapp03   | banner | /home/banner/jenkins  |

Follow the same process as above, adjusting host, username, and directory for each.

---

### Step 4: Fix "Unknown java command" Issue

If a node shows **offline** with log message:
```
unknown java command
```
then Java must be installed on the app servers.

**Install Java and create Jenkins directory:**

```bash
ssh tony@stapp01
sudo yum install -y java-11-openjdk
mkdir -p /home/tony/jenkins
chown -R tony:tony /home/tony/jenkins

ssh steve@stapp02
sudo yum install -y java-11-openjdk
mkdir -p /home/steve/jenkins
chown -R steve:steve /home/steve/jenkins

ssh banner@stapp03
sudo yum install -y java-11-openjdk
mkdir -p /home/banner/jenkins
chown -R banner:banner /home/banner/jenkins
```

Then return to Jenkins and **launch agent** again.

---

### Step 5: Verify Node Connectivity

1. Go to **Manage Jenkins → Nodes**.
2. Ensure all nodes show as **Online**.
3. Optionally, create a test job to run on each node:

**Example command:**
```bash
echo "Hello from $(hostname)"
```

---

## Final Expected Configuration

| Node Name     | Label  | Remote Directory        | Hostname | User   | Status  |
|----------------|--------|-------------------------|-----------|--------|----------|
| App_server_1   | stapp01 | /home/tony/jenkins     | stapp01   | tony   | ✅ Online |
| App_server_2   | stapp02 | /home/steve/jenkins    | stapp02   | steve  | ✅ Online |
| App_server_3   | stapp03 | /home/banner/jenkins   | stapp03   | banner | ✅ Online |

---

## Troubleshooting Tips

- If SSH fails: verify username, key permissions, and connectivity.
- If Java not found: install OpenJDK as shown above.
- If directory permission issues: ensure Jenkins user has ownership of `/home/<user>/jenkins`.

---

✅ **Task Completed:** All app servers are successfully configured as Jenkins slave nodes and are online.