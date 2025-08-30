# Day 26: Git Manage Remotes

## Question

The xFusionCorp development team added updates to the project that is maintained under `/opt/media.git` repo and cloned under `/usr/src/kodekloudrepos/media`. Recently some changes were made on Git server that is hosted on `Storage server` in `Stratos DC`. The DevOps team added some new Git remotes, so we need to update remote on `/usr/src/kodekloudrepos/media` repository as per details mentioned below:

**Task:**

a. In `/usr/src/kodekloudrepos/media` repo add a new remote `dev_media` and point it to `/opt/xfusioncorp_media.git` repository.

b. There is a file `/tmp/index.html` on same server; copy this file to the repo and add/commit to master branch.

c. Finally push `master` branch to this new remote origin.

---

## Solution

1. **Switch to Storage Server**

(Usually in KodeKloud you select the "Storage Server" from the terminal options.)

```bash
ssh natasha@ststor01
# password: Bl@kW
```

---

2. **Go to the repository**

```bash
cd /usr/src/kodekloudrepos/media
```

---

3. **Add the new remote**

Add the remote `dev_media` pointing to `/opt/xfusioncorp_media.git`:

```bash
git remote add dev_media /opt/xfusioncorp_media.git
```
👉 You can verify with:

```bash
git remote -v
```
You should see something like:

```bash
dev_media   /opt/xfusioncorp_media.git (fetch)
dev_media   /opt/xfusioncorp_media.git (push)
```
---

4. **Copy the file**

Copy `/tmp/index.html` into the repository:

```bash
cp /tmp/index.html .
```

---

5. **Add and commit**

```bash
git add index.html
git commit -m "Add index.html file"
```

---

6. **Push changes to the new remote**

Push the `master` branch to the new remote `dev_media`:

```bash
git push dev_media master
```

---

✅ Done!
This will satisfy all requirements:
1. Remote dev_media added.
2. index.html added and committed.
3. Pushed to the new remote.

---