# Day 68: Set Up Jenkins Server

## Question

The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:

**Task:**

1. Install `Jenkins` on the jenkins server using the `yum` utility only, and start its service.

  - If you face a timeout issue while starting the Jenkins service, refer to [this](https://www.jenkins.io/doc/book/system-administration/systemd-services/#starting-services).

2. Jenkin's admin user name should be `theadmin`, password should be `Adm!n321`, full name should be `Mark` and email should be `mark@jenkins.stratos.xfusioncorp.com`.

`Note:` 

1. To access the `jenkins` server, connect from the jump host using the `root` user with the password `S3curePass`.

2. After Jenkins server installation, click the `Jenkins` button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.

---

## 🧩 Step-by-Step Solution

1. **Connect to the Jenkins Server**

From the **jump host**:

```bash
ssh root@jenkins
# password: S3curePass
```
---

2. **Install Java**

Jenkins requires Java (OpenJDK 11 or 17 or 21). Install it using `yum`:

```bash
sudo yum install fontconfig java-21-openjdk
```
Check the version:

```bash
java -version
```
---

3. **Add Jenkins Repository**

Use yum to add the official Jenkins repo:

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
```
---

4. **Install Jenkins**

```bash
sudo yum install -y jenkins
sudo systemctl daemon-reload
```
---

5. **Enable and Start Jenkins**

```bash
systemctl enable jenkins
systemctl start jenkins
```
Check status:

```bash
systemctl status jenkins
```
---

6. **Access Jenkins UI**

Open your browser and navigate to:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```
Copy that password and paste it into the browser.

---

8. **Install Suggested Plugins**

Select:

  ***“Install suggested plugins”***

Wait for all plugins to finish installing.

---

9. Create the Admin User

When prompted, fill in these details:

Field :	      Value
Username :	  theadmin
Password :	  Adm!n321
Full Name	:   Mark
Email :	      mark@jenkins.stratos.xfusioncorp.com

Then click **Save and Continue**.

---

10. **Instance Configuration**

Just click:

  ***“Save and Finish” → “Start using Jenkins”***

---

✅ Jenkins setup complete!

You can verify the Jenkins service is running:

```bash
systemctl status jenkins
```