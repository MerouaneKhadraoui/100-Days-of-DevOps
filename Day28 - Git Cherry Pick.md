# Day 28: Git Cherry Pick

## Question

The Nautilus application development team has been working on a project repository `/opt/ecommerce.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with the DevOps team:

**Task:**

There are two branches in this repository, `master` and `feature`. One of the developers is working on the `feature` branch and their work is still in progress, however they want to merge one of the commits from the feature `branch` to the `master` branch, the message for the commit that needs to be merged into `master` is `Update info.txt`. Accomplish this task for them, also remember to push your changes eventually.

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
cd /usr/src/kodekloudrepos/ecommerce
```

---

3. **Check branches**

```bash
git branch
```
You should see `master` and `feature`.

---

4. **Find the commit hash in the feature branch**

```bash
git checkout feature
git log --oneline
```
Look for the commit with the message:
`Update info.txt`
Copy its commit hash (e.g., `abc1234`).

---

5. **Switch to master branch**

```bash
git checkout master
#or
git switch master
```

---

6. **Cherry-pick the commit**

```bash
git cherry-pick <commit-hash>
```
Replace `<commit-hash>` with the hash you copied.

---

7. **Push the changes**

```bash
git push origin master
```

---

✅ **Final Command Flow**

```bash
cd /usr/src/kodekloudrepos/ecommerce
git checkout feature
git log --oneline    # find hash of "Update info.txt"
git checkout master
git cherry-pick <commit-hash>
git push origin master
```

---