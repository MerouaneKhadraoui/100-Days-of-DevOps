# Day 69: Install Jenkins Plugins

## Question

The Nautilus DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task

**Task:**

1. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

2. Once logged in, install the `Git` and `GitLab` plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`.

`Note:` 

1. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding.

2. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.

---

## ✅ Step 1: Access the Jenkins Web UI

1. On the KodeKloud lab environment, click on the **“Jenkins”** button at the top bar.
This will open the Jenkins interface in a new tab or window.

2. Log in with the following credentials:

- **Username:** `admin`
- **Password:** `Adm!n321`

---

## ✅ Step 2: Go to Plugin Management

1. From the Jenkins dashboard, click on **“Manage Jenkins”** (usually on the left sidebar).
2. Then click on **“Plugins”** or **“Manage Plugins”** (the wording may differ slightly depending on Jenkins version).

---

## ✅ Step 3: Install Required Plugins

1. In the plugin manager, click on the **“Available”** tab.

2. In the search box, type **Git**.

- Check the box next to **Git Plugin**.

3. Then type **GitLab**.

- Check the box next to **GitLab Plugin** (or “GitLab Integration Plugin” depending on your version).

4. Scroll down and click the **“Install without restart”** button.

---

## ✅ Step 4: Restart Jenkins (if required)

1. During or after installation, you might see a checkbox labeled:
**“Restart Jenkins when installation is complete and no jobs are running”**

2. Check this box to automatically restart Jenkins once both plugins are installed.

⚠️ If Jenkins does not restart automatically:

- Go to the Jenkins URL (e.g., `http://<IP>:8080/restart`)
- Or restart manually from the system:

```bash
sudo systemctl restart jenkins
```
---

## ✅ Step 5: Verify Plugin Installation

1. Wait until Jenkins restarts and the login page reappears.
2. Log in again (`admin` / `Adm!n321`).
3. Go back to **Manage Jenkins** → **Plugins** → **Installed**.
4. Verify that both:
- **Git Plugin**
- **GitLab Plugin**
are listed as **Installed**.

---

## 🧾 Optional — Command-Line Verification

If you have terminal access to the Jenkins server, you can confirm via:

```bash
ls /var/lib/jenkins/plugins | grep -E 'git|gitlab'
```
You should see plugin .jpi files like:

```bash
git.jpi
gitlab-plugin.jpi
```
---

## ✅ Final Validation

- Both plugins are visible under “Installed plugins”.
- Jenkins dashboard is accessible after restart.
- No installation errors.