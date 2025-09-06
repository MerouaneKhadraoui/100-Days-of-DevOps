# Day 34: Git Hook

## Question

The Nautilus application development team was working on a git repository `/opt/cluster.git` which is cloned under `/usr/src/kodekloudrepos` directory present on `Storage server` in `Stratos DC`. The team want to setup a hook on this repository, please find below more details:

**Task:**

- Merge the `feature` branch into the `master` branch`, but before pushing your changes complete below point.

- Create a `post-update` hook in this git repository so that whenever any changes are pushed to the `master` branch, it creates a release tag with name `release-2023-06-15`, where `2023-06-15` is supposed to be the current date. For example if today is `20th June, 2023` then the release tag must be `release-2023-06-20`. Make sure you test the hook at least once and create a release tag for today's release.

- Finally remember to push your changes.

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
cd /usr/src/kodekloudrepos/cluster
```

---

3. **Merge feature into master**

```bash
git checkout master
git pull origin master        # update master first
git merge feature             # merge feature branch into master
```
Resolve any merge conflicts if they appear, then commit.

---

4. **Create the post-update hook**

Hooks live inside `.git/hooks/`.

- Navigate to the hooks folder:

```bash
cd .git/hooks
```

- Create a post-update hook script:

```bash
vi post-update
```

- Add the following content:

```bash
#!/bin/bash
# Git post-update hook to auto-create release tags

# Get current date in YYYY-MM-DD format
DATE=$(date +%F)

# Only run when master branch is updated
branch=$(git rev-parse --symbolic --abbrev-ref HEAD)

if [ "$branch" = "master" ]; then
    TAG="release-$DATE"
    git tag -a "$TAG" -m "Automated release for $DATE"
    git push origin "$TAG"
fi
```

- Make it executable:

```bash
chmod +x post-update
```
---

5. **Test the hook**

Now push your merged master branch. The hook should automatically create a tag.

```bash
git push origin master
```

---

6. **Verify the release tag**

Check if the release tag exists on the remote:

```bash
git ls-remote --tags origin
```
You should see something like:

```bash
refs/tags/release-2025-09-06
```
---

✅ At this point:

- feature is merged into master
- A post-update hook auto-creates release-YYYY-MM-DD tag
- The tag has been pushed to the remote repo