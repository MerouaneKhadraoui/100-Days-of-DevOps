# Day 31: Git Stash

## Question

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/blog` present on `Storage server` in `Stratos DC`. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:

**Task:**

Look for the stashed changes under `/usr/src/kodekloudrepos/blog` git repository, and restore the stash with `stash@{1}` identifier. Further, commit and push your changes to the origin.

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
cd /usr/src/kodekloudrepos/blog
```

---

3. **Check the stashes available**

```bash
git stash list
```
You should see something like:

```sql
stash@{0}: WIP on main: 123abc some commit msg
stash@{1}: WIP on main: 456def another commit msg
```
---

4. **Apply the required stash**

```bash
git stash pop 1
# or
git stash apply stash@{1}
```

---

5. **Check the changes**

```bash
git status
```
You should now see the files restored from the stash.

---

6. **Stage the changes**

```bash
git add .
```
---

7. **Commit the changes**

```bash
git commit -m "Restored changes from stash 1"
```
---

8. **Push the commit to origin**

```bash
git push origin master
# or
git push
```
---

🔑 **Final Commands Summary**

```bash
cd /usr/src/kodekloudrepos/blog
git stash list
git stash pop 1
git add .
git commit -m "Restored changes from stash 1"
git push
```