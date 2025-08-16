# Day 11: Install and Configure Tomcat Server

## Question

The `Nautilus` application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in `Stratos DC`. After an internal team meeting, they have decided to use the `tomcat` application server. Based on the requirements mentioned below complete the task:

**Task:**  

a. Install `tomcat` server on `App Server 3`.

b. Configure it to run on port `8085`.

c. There is a `ROOT.war` file on `Jump host` at location `/tmp`.


Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp03:8085

---

## Solution

1. **Understanding the Setup**

- App Server 1: where the website files exist under `/var/www/html/official`.
- Backup Server: Nautilus Backup Server.
- Goal:

  - Zip the directory.
  - Save it locally under `/backup/`.
  - Copy it to Backup Server `/backup/` without asking for a password.

---

2. **Passwordless SSH Setup**

Before the script can run without prompting for a password, you need to configure SSH key-based authentication.

On App Server 1:

```bash
# Login as the correct user, e.g., tony
ssh-keygen -t rsa
# Press enter for all prompts (no passphrase)
ssh-copy-id clint@stbkp01
```

Now test:

```bash
ssh clint@stbkp01
```

If it logs in without asking for a password, you're good.

---

3. **Script Creation**

Create `/scripts/official_backup.sh` on App Server 1:

```bash
sudo mkdir -p /scripts
sudo vi /scripts/official_backup.sh
```

Paste this:

```bash
#!/bin/bash

# Variables
SOURCE_DIR="/var/www/html/official"
BACKUP_NAME="xfusioncorp_official.zip"
LOCAL_BACKUP_DIR="/backup"
REMOTE_USER="clint"       # Adjust as needed
REMOTE_HOST="stbkp01"     # Adjust as needed
REMOTE_BACKUP_DIR="/backup"

# Create local backup directory if not exists
mkdir -p "$LOCAL_BACKUP_DIR"

# Create the zip archive
zip -r "${LOCAL_BACKUP_DIR}/${BACKUP_NAME}" "$SOURCE_DIR"

# Copy to Nautilus Backup Server
scp "${LOCAL_BACKUP_DIR}/${BACKUP_NAME}" "${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP_DIR}/"

# Completion message
echo "Backup completed and copied to Nautilus Backup Server."
```

---

4. **Permissions**

```bash
sudo chmod +x /scripts/official_backup.sh
sudo chown tony:tony /scripts/official_backup.sh   # Replace with actual user
```

---

5. **Test the Script**

```bash
/scripts/official_backup.sh
```

Check:

```bash
ls /backup/                     # Local backup exists
ssh clint@stbkp01 ls /backup/   # Remote backup exists
```

---

✅ This meets all requirements:

- Backs up /var/www/html/official into xfusioncorp_official.zip
- Stores locally in /backup
- Sends to /backup/ on Nautilus Backup Server
- No password prompt because of SSH key setup
- Usable by the correct server user