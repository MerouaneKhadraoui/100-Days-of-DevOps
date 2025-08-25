# Day 22: Clone Git Repository on Storage Server

## Question

The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the `Storage Server` in the Stratos DC. Follow the provided details to clone the repository:

**Task:**

1. The repository to be cloned is located at `/opt/official.git`

2. Clone this Git repository to the `/usr/src/kodekloudrepos` directory. Ensure no modifications are made to the repository during the cloning process.

---

## Solution


**Step 1 - Switch to Storage Server**

In KodeKloud Engineer, you first connect to the Storage Server:

```bash
ssh natasha@ststor01
# password: Bl@kW
```

---

**Step 2 - Access the directory**

```bash
cd /usr/src/kodekloudrepos/
```

---

**Step 3 - Clone the Repository**

The repository to be cloned is located at `/opt/official.git`:

```bash
sudo git clone /opt/official.git
```

---

**Step 4 - Verify Repository**

Check that it is indeed a repo:

```bash
ls -a /usr/src/kodekloudrepos/official/
```

You should see files like:

.  ..  .git

---
