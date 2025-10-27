# Day 76: Jenkins Project Security

## Question

The xFusionCorp Industries has recruited some new developers. There are already some existing jobs on Jenkins and two of these new developers need permissions to access those jobs. The development team has already shared those requirements with the DevOps team, so as per details mentioned below grant required permissions to the developers.

**Task:**

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`

1. There is an existing Jenkins job named `Packages`, there are also two existing Jenkins users named `sam` with password `sam@pass12345` and `rohan` with password `rohan@pass12345`.

2. Grant permissions to these users to access `Packages` job as per details mentioned below:

  a. Make sure to select `Inherit permissions from parent ACL` under `inheritance strategy` for granting permissions to these users.

  b. Grant mentioned permissions to `sam` user : `build`, `configure` and `read`.

  c. Grant mentioned permissions to `rohan` user : `build`, `cancel`, `configure`, `read`, `update` and `tag`.


`Note:` 

1. Please do not modify/alter any other existing job configuration.

2. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

---

### Solution

## Objective
Grant specific permissions to Jenkins users `sam` and `rohan` for the existing job named `Packages`.

---

## Login Information
- **Jenkins URL:** (Click the Jenkins button in the lab environment)
- **Username:** `admin`
- **Password:** `Adm!n321`

---

## Step-by-Step Solution

### Step 1: Login to Jenkins
1. Open Jenkins from the lab environment.
2. Login using the credentials:
   ```
   Username: admin
   Password: Adm!n321
   ```

---

### Step 2: Verify Authorization Strategy
1. Go to **Manage Jenkins → Configure Global Security**.
2. Ensure **Project-based Matrix Authorization Strategy** is selected under **Authorization**.
3. Save the configuration if needed.

---

### Step 3: Configure the Job
1. Navigate to **Jenkins Dashboard → Packages**.
2. Click **Configure** on the left sidebar.

---

### Step 4: Enable Project-based Security
1. Scroll to the **Project-based Matrix Authorization Strategy** section.
2. Check the box: ✅ **Enable project-based security**.
3. Under **Inheritance strategy**, select:
   ```
   Inherit permissions from parent ACL
   ```

---

### Step 5: Add Users and Assign Permissions

#### For user `sam`
- Add user: `sam`
- Grant these permissions:
  - ✅ **Job → Build**
  - ✅ **Job → Configure**
  - ✅ **Job → Read**

#### For user `rohan`
- Add user: `rohan`
- Grant these permissions:
  - ✅ **Job → Build**
  - ✅ **Job → Cancel**
  - ✅ **Job → Configure**
  - ✅ **Job → Read**
  - ✅ **Job → Update**
  - ✅ **Job → Tag**

> ⚠️ Do **not** modify or remove any existing job configurations or user permissions.

---

### Step 6: Save Configuration
Click **Save** or **Apply** at the bottom of the job configuration page.

---

### Step 7: Verify Permissions (Optional)
1. Logout as `admin`.
2. Login as `sam` → confirm access to `Packages` job with build/configure/read rights.
3. Login as `rohan` → confirm extended permissions (build, cancel, configure, read, update, tag).

---

## Expected Result
| User  | Permissions |
|--------|--------------|
| **sam** | build, configure, read |
| **rohan** | build, cancel, configure, read, update, tag |

**Inheritance:** Enabled from parent ACL.

---

✅ **Task Completed Successfully**