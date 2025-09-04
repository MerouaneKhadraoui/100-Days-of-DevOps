# Day 32: Git Rebase

## Question

The Nautilus application development team has been working on a project repository `/opt/official.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:

**Task:**

One of the developers is working on `feature` branch and their work is still in progress, however there are some changes which have been pushed into the `master` branch, the developer now wants to `rebase` the `feature` branch with the `master` branch without loosing any data from the feature branch, also they don't want to add any `merge commit` by simply merging the `master` branch into the `feature` branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.

---

## Solution

1. **Switch to Storage Server**

(Usually in KodeKloud you select the "Storage Server" from the terminal options.)

```bash
ssh natasha@ststor01
# password: Bl@kW
```

---

2. **Go to the repo path**

```bash
cd /usr/src/kodekloudrepos/official
```

---

3. **Check the current branches**

```bash
git branch
```
You should see `master` and `feature`.

---

4. **Switch to the `feature` branch**

```bash
git swith feature
```

---

5. **Fetch the latest changes from the repo (to make sure master is updated)**

```bash
git fetch origin
```

---

6. **Rebase `feature` on top of `master`**

```bash
git rebase origin/master
```
- If there are conflicts, Git will pause.
- Resolve conflicts in the files, then:
```bash
git add <file>
git rebase --continue
```
- Repeat until the rebase is done.
---

7. **Verify the rebase**

```bash
git log --oneline --graph --decorate --all
```
You should see `feature` commits now on top of `master` commits (without a merge commit).

---

8. **Push the updated `feature` branch (with force, since history was rewritten by rebase)**

```bash
git push origin feature --force
```
---

✅ That’s it! Now the `feature` branch contains all updates from `master`, the developer’s work is preserved, and no merge commit was added.