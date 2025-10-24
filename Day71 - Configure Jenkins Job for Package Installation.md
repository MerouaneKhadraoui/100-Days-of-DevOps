# Day 71: Configure Jenkins Job for Package Installation

## Question

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

**Task:**

1. Access the Jenkins UI by clicking on the `Jenkins` button in the top bar. Log in using the credentials: username `admin` and password `Adm!n321`.

2. Create a new Jenkins job named `install-packages` and configure it with the following specifications:

    - Add a string parameter named `PACKAGE`.
    - Configure the job to install a package specified in the `$PACKAGE` parameter on the `storage server` within the `Stratos Datacenter`.

`Note:` 

1. Ensure to install any required plugins and restart the Jenkins service if necessary. Opt for `Restart Jenkins when installation is complete and no jobs are running` on the plugin installation/update page. Refresh the UI page if needed after restarting the service.

2. Verify that the Jenkins job runs successfully on repeated executions to ensure reliability.

3. Capture screenshots of your configuration for documentation and review purposes. Alternatively, use screen recording software like `loom.com` for comprehensive documentation and sharing.

---

## ✅ Goal

Create a Jenkins job called `install-packages` that installs a Linux package (given as a parameter) on the **storage serve**r in the **Stratos Datacenter**.

---

## 🧩 Step-by-Step Solution

## 1. Access Jenkins

1. Click the **“Jenkins”** button in the KodeKloud lab environment.

2. Login using:

- **Username:** `admin`
- **Password:** `Adm!n321`

---

## 2. Create a New Jenkins Job

1. On Jenkins dashboard → click “**New Item**”.
2. Enter the job name:

```bash
install-packages
```

3. Select “**Freestyle project**” → click **OK**.

---

## 3. Add a Parameter

1. In the job configuration page, check “**This project is parameterized**”.
2. Click **Add Parameter** → **String Parameter**.
3. Fill in:
    - **Name**: `PACKAGE`
    - **Default Value (optional)**: `httpd` (or `nginx`, `git`, etc.)
    - **Description**: “Package name to install on storage server”

---

## 4. Add the Build Step to Run a Command on Remote Storage Server

We need the job to connect to the **storage server** and install the specified package.

There are two common methods:

- Using the **SSH plugin** (preferred)
- Using plain SSH commands (if plugin not available)

---

## 4A. (If SSH Plugin Available)

1. Make sure the “**SSH Agent**” or “**Publish over SSH**” plugin is installed:
    - Go to **Manage Jenkins** → **Plugins** → **Available tab**.
    - Search for **SSH** and install:
        - ✅ ***Publish Over SSH***
        - ✅ ***SSH Agent Plugin***
    - Restart Jenkins when prompted.

2. Configure SSH credentials:
    - Go to **Manage Jenkins** → **Configure System**
    - Scroll to **Publish over SSH**
    - Add a new SSH Server:
        - **Name**: `storage-server`
        - **Hostname**: `ststor01` (storage server hostname from the lab)
        - **Username**: `natasha` (or as provided)
        - **Password / Key**: use the same credentials available for SSH access in the lab (usually `natasha` / `*****`)
    - Test the connection (click **Test Configuration**).

3. Back to the `install-packages` job → **Add build step** → **Send files or execute commands over SSH**
    - Select **storage-server**
    - In the **Exec command** box, add:
    ```bash
    echo 'Bl@kW' | sudo -S yum install -y $PACKAGE
    ```
    ***(or `apt-get install -y $PACKAGE` if it’s Ubuntu)***

---

## 4B. (If No Plugin Available – Use SSH Command Directly)

1. Click **Add build step** → **Execute shell**
2. Paste the command:

```bash
ssh -o StrictHostKeyChecking=no natasha@ststor01 "sudo yum install -y ${PACKAGE}"
```
**(Replace `yum` with `apt-get` if needed)**

⚠️ Ensure that Jenkins can SSH into the storage server without a password (configure SSH key if required).

---

## 5. Save and Run the Job

1. Click **Save**.
2. Click **Build with Parameters**.
3. Enter the package name, e.g.: `httpd`
4. Click **Build**.
5. Monitor the console output:
    - You should see Jenkins connecting to the storage server and installing the package successfully.

---

## 6. Verify Reliability

Run the job multiple times with different packages to confirm it works consistently:

```ini
PACKAGE = git
PACKAGE = wget
PACKAGE = tree
```
Check the console output — Jenkins should install each package without failure (or skip if already installed).

---

## ✅ Verification

SSH into the storage server manually to confirm the package installation:

```bash
ssh natasha@ststor01
rpm -qa | grep httpd
# or
dpkg -l | grep httpd
```

---

## 🧠 Bonus (Best Practice)

To make it more robust, add a check before installing:

```bash
if ! rpm -qa | grep -qw $PACKAGE; then
  sudo yum install -y $PACKAGE
else
  echo "$PACKAGE is already installed."
fi
```

---

## 🎯 Final Expected Behavior

- Job name: `install-packages`
- Parameter: `PACKAGE`
- Build step: SSH command to install `$PACKAGE` on storage server
- Works reliably on repeated runs
- Uses Jenkins automation to manage infrastructure package installation