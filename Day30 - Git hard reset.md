# Day 30: Git hard reset

## Question

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/games` present on `Storage server` in `Stratos DC`. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the `HEAD` and the branch itself to a commit with message `add data.txt file`. Find below more details:

**Task:**

1. In `/usr/src/kodekloudrepos/games` git repository, reset the git commit history so that there are only two commits in the commit history i.e `initial commit` and `add data.txt file`.

2. Also make sure to push your changes.

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
cd /usr/src/kodekloudrepos/games
```

---

3. **Check git log to identify the commit**

```bash
git log --oneline
```
You should see something like:

```sql
a1b2c3d add data.txt file
x9y8z7  another commit
...
1234567 initial commit
```
Copy the commit hash of the `add data.txt file` commit (let's say it's `a1b2c3d`).

---

4. **Hard reset the branch to that commit**

```bash
git reset --hard a1b2c3d
```

---

5. **Remove later commits from history by force pushing**

```bash
git push origin HEAD --force
```
⚠️ This rewrites remote history, which is what they want here.

---

6. **Verify the commit history now has only two commits:**

```bash
git log --oneline
```
Expected output:

```sql
a1b2c3d add data.txt file
1234567 initial commit
```

---

✅ Done! Now the repo is cleaned, with only `initial commit` and `add data.txt file` left in history.