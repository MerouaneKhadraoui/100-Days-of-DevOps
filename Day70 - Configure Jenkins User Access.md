# Day 70: Configure Jenkins User Access

## Question

The Nautilus team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:

**Task:**

1. Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login with username `admin` and password `Adm!n321`.

2. Create a jenkins user named `jim` with the password `GyQkFRVNr3`. Their full name should match `Jim`.

3. Utilize the `Project-based Matrix Authorization Strategy` to assign `overall read` permission to the `jim` user.

4. Remove all permissions for `Anonymous` users (if any) ensuring that the `admin` user retains overall `Administer` permissions.

5. For the existing job, grant `jim` user only `read` permissions, disregarding other permissions such as Agent, SCM etc.

`Note:` 

1. You may need to install plugins and restart Jenkins service. After plugins installation, select `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page.

2. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding. Avoid clicking `Finish` immediately after restarting the service.

3. Capture screenshots of your configuration for review purposes. Consider using screen recording software like `loom.com` for documentation and sharing.

---

## ✅ Step-by-Step Solution

1. **Login to Jenkins**

- Open Jenkins via the top navigation bar (`Jenkins` button in the lab interface).
- Login credentials:
    - Username: `admin`
    - Password: `Adm!n321`

---

2. **Install Required Plugin**

    To use **Project-based Matrix Authorization Strategy**, make sure it’s installed:

    1. Go to **Manage Jenkins** → **Plugins** → **Available plugins tab**.
    2. Search for: **"Matrix Authorization Strategy"**.
    3. Check the box next to it, then click **Install without restart** (or **Install and restart**, depending on the UI).
    4. Once installed, check the box **“Restart Jenkins when installation is complete and no jobs are running.”**

    🕓 Wait for Jenkins to restart, then re-login with the admin credentials.

---

3. **Create User “jim”**

    1. Go to **Manage Jenkins** → **Users** → **Create User**.
    2. Fill in the following:
        - **Username**: `jim`
        - **Password**: `GyQkFRVNr3`
        - **Confirm password**: `GyQkFRVNr3`
        - **Full name**: `Jim`
        - **Email address**: (optional)
    3. Click **Create User**.

✅ User `jim` is now created.

---

4. **Enable Matrix Authorization Strategy**

    1. Navigate to **Manage Jenkins** → **Configure Global Security**.
    2. Under **Authorization**, select:
        - `Project-based Matrix Authorization Strategy`.
    3. A permissions grid will appear.
---

5. **Set Permissions**

    - In the grid:

    User	Overall	Job	Agent	SCM	View	etc.
    admin	✅ Administer	—	—	—	—	—
    jim	✅ Read	—	—	—	—	—
    Anonymous	❌ Remove all permissions					

    ⚠️ Make sure **admin** keeps “Overall → Administer” checked, otherwise you’ll lock yourself out.

    - Scroll down and click **Save**.
---

6. **Apply Job-Specific Permissions**

    Now, restrict job access for `jim`.

    1. Go to the **existing job** (usually something like `devops-project` or any listed job).
    2. Click **Configure**.
    3. Scroll down to the bottom and check **“Enable project-based security.”**
    4. In the permissions grid:
        - Add user: `jim`
        - Grant only:
            - **Job** → **Read**
        - Ensure no other boxes are checked.

    5. Save the job configuration.
---

7. **Verify the Configuration**

    1. Logout from admin.
    2. Login as:
        - **Username**: `jim`
        - **Password**: `GyQkFRVNr3`
    3. Check that:
        - You can **see** the job.
        - You **cannot modify or run** the job.
        - You can **only read/view**.

---

## ✅ Summary of What You Did

- Installed the Matrix Authorization plugin.
- Created user `jim`.
- Enabled **Project-based Matrix Authorization Strategy**.
- Gave `jim` **Overall** → **Read** permission.
- Removed all **Anonymous** permissions.
- Granted `jim` **Job** → **Read** access only on one project.
- Verified permissions.