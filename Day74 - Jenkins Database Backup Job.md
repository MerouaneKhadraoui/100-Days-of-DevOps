# Day 74: Jenkins Database Backup Job

## Question

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:

**Task:**

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a Jenkins job named `database-backup`.

2. Configure it to take a database dump of the `kodekloud_db01` database present on the `Database server` in `Stratos Datacenter`, the database user is `kodekloud_roy` and password is `asdfgdsd`.

3. The dump should be named in `db_$(date +%F).sql` format, where `date +%F` is the current date.

4. Copy the `db_$(date +%F).sql` dump to the `Backup Server` under location `/home/clint/db_backups`.

5. Further, schedule this job to run periodically at `*/10 * * * *` (please use this exact schedule format).

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

---

# SOLUTION

## Objective

Automate database backup using Jenkins by creating a scheduled job that takes a MySQL dump from the Database Server and copies it to the Backup Server.

---

## Steps

### 1. Login to Jenkins
- Click on the **"Jenkins"** button on the top bar.
- Login using the credentials:
  - **Username:** `admin`
  - **Password:** `Adm!n321`

---

### 2. Create the Jenkins Job
1. Click **"New Item"**.
2. Enter the name **`database-backup`**.
3. Select **"Freestyle project"**.
4. Click **OK**.

---

### 3. Configure the Build Step

#### Add a Build Step
1. Scroll to the **Build** section.
2. Click **"Add build step" → "Execute shell"**.
3. Add the following script:

```bash
#!/bin/bash

# Database details
DB_NAME="kodekloud_db01"
DB_USER="kodekloud_roy"
DB_PASS="asdfgdsd"
BACKUP_FILE="db_$(date +%F).sql"

# Take database dump (on the Database server)
ssh tony@stapp01 "mysqldump -u${DB_USER} -p${DB_PASS} ${DB_NAME} > /tmp/${BACKUP_FILE}"

# Copy dump file to Backup server
scp tony@stapp01:/tmp/${BACKUP_FILE} clint@stbkp01:/home/clint/db_backups/

# Clean up temporary dump file
ssh tony@stapp01 "rm -f /tmp/${BACKUP_FILE}"
```

---

### 4. Schedule the Job
1. Scroll to **"Build Triggers"**.
2. Check **"Build periodically"**.
3. Enter the schedule:
   ```
   */10 * * * *
   ```
   This runs the job every **10 minutes**.

---

### 5. Save & Run
- Click **"Save"**.
- Click **"Build Now"** to test the job.
- Check the **Console Output** for successful execution.

---

### 6. Verify Backup on Backup Server
SSH into the backup server and verify the file:

```bash
ssh clint@stbkp01
ls -l /home/clint/db_backups/
```

✅ You should see:
```
db_YYYY-MM-DD.sql
```

---

## Summary

| Step | Action | Command/Setting |
|------|---------|-----------------|
| 1 | Create Freestyle job | `database-backup` |
| 2 | Add build step | *Execute shell* |
| 3 | Script | See above |
| 4 | Schedule | `*/10 * * * *` |
| 5 | Verify | `/home/clint/db_backups/db_YYYY-MM-DD.sql` |

---

### Optional: Jenkins Pipeline Version

```groovy
pipeline {
    agent any
    triggers {
        cron('*/10 * * * *')
    }
    stages {
        stage('Database Backup') {
            steps {
                script {
                    def date = sh(script: "date +%F", returnStdout: true).trim()
                    def backupFile = "db_${date}.sql"
                    sh '''
                    ssh tony@stapp01 "mysqldump -ukodekloud_roy -pasdfgdsd kodekloud_db01 > /tmp/${backupFile}"
                    scp tony@stapp01:/tmp/${backupFile} clint@stbkp01:/home/clint/db_backups/
                    ssh tony@stapp01 "rm -f /tmp/${backupFile}"
                    '''
                }
            }
        }
    }
}
```

---

**Author:** Merouane KHADRAOUI  
**Platform:** KodeKloud  
**Task:** Jenkins Database Backup Automation (Task 4)