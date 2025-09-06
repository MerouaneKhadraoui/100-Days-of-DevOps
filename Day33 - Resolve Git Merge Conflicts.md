# Day 33: Resolve Git Merge Conflicts

## Question

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:

**Task:**

SSH into `storage server` using user `max` and password `Max_pass123`. Under `/home/max` you will find the `story-blog` repository. Try to push the changes to the origin repo and fix the issues. The `story-index.txt` must have titles for all 4 stories. Additionally, there is a typo in `The Lion and the Mooose` line where `Mooose` should be `Mouse`.

Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` page. You can login to `Gitea` server from UI using username `sarah` and password `Sarah_pass123` or username `max` and password `Max_pass123`.

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