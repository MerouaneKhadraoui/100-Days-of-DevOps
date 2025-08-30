# Day 27: Git Revert Some Changes

## Question

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/media` present on `Storage server` in `Stratos DC`. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

**Task:**

1. In `/usr/src/kodekloudrepos/media` git repository, revert the latest commit `( HEAD )` to the previous commit (JFYI the previous commit hash should be with `initial commit` message ).

2. Use `revert media` message (please use all small letters for commit message) for the new revert commit.

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

3. **Check the commit history to confirm**

```bash
git log --oneline
```
👉 You should see something like:

```bash
abc123 Latest changes
def456 initial commit
```

---

4. **Revert the latest commit**

```bash
git revert HEAD --no-edit
```

---

5. **Revert message**

```bash
git commit --amend -m "revert media"
```

---

6. **Verify the history**

```bash
git log --oneline
```
👉 The latest commit should now say:

```sql
xyz789 revert media
def456 initial commit
```

---

✅ That completes the task.

The repo is now reverted to the state of the initial commit, with a new commit labeled "revert media".

---