# Day 10: Linux Bash Scripts

## Question

The production support team of `xFusionCorp Industries` is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on `App Server 1` in `Stratos Datacenter`, and they need to create a bash script named `official_backup.sh` which should accomplish the following tasks. (Also remember to place the script under `/scripts` directory on `App Server 1`).

**Task:**  

a. Create a zip archive named `xfusioncorp_official.zip` of `/var/www/html/official` directory.


b. Save the archive in `/backup/` on `App Server 1`. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on `Nautilus Backup Server`.


c. Copy the created archive to `Nautilus Backup Server` server in `/backup/` location.


d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of `App Server 1`) must be able to run it.

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