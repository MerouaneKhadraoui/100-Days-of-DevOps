# Day 25: Git Merge Branches

## Question

The Nautilus application development team has been working on a project repository `/opt/media.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:

**Task:**

Create a new branch `xfusion` in `/usr/src/kodekloudrepos/media` repo from `master` and copy the `/tmp/index.html` file (present on `storage server` itself) into the repo. Further, `add/commit` this file in the new branch and merge back that branch into `master` branch. Finally, push the changes to the origin for both of the branches.

---

## Solution

1. **Switch to Storage Server**

(Usually in KodeKloud you select the "Storage Server" from the terminal options.)

```bash
ssh natasha@ststor01
# password: Bl@kW
```

---

2. **Go to the repo directory**

```bash
cd /usr/src/kodekloudrepos/media
```

---

3. **Check you are on `master` branch**

```bash
git status
git branch
```
If not on `master`, switch to it:

```bash
git checkout master
#or
git switch master
```

---

4. **Create and switch to new branch `xfusion`**

```bash
git checkout -b xfusion
```

---

5. **Copy the file from `/tmp` into the repo**

```bash
cp /tmp/index.html .
```

---

6. **Stage and commit the file**

```bash
git add index.html
git commit -m "Add index.html to xfusion branch"
```

---

7. **Switch back to `master`**

```bash
git checkout master
```

---

8. **Merge branch `xfusion` into `master`**

```bash
git merge xfusion
```

---

9. **Push both branches to origin**

```bash
git push origin master
git push origin xfusion
```

---

✅ At this point:

- You created the `xfusion` branch from `master`.
- Added `index.html` from `/tmp` to it.
- Merged the branch back into `master`.
- Pushed both branches (`master` and `xfusion`) to the remote origin.

---